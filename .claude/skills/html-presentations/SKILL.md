# HTML Presentations

Convert a markdown spec file into an HTML presentation using the Poimandres-themed template.

## Files

- **Template**: `{SKILLBASE}/template/index.html`
- **Example spec**: `{SKILLBASE}/examples/spec.md`
- **Build script**: `{SKILLBASE}/scripts/create_standalone.sh`

## Instructions

1. Read the spec markdown file (e.g., `spec.md`)

2. Read the template at `{SKILLBASE}/template/index.html`

3. Parse the spec file:
   - Split on `* * *` to get individual slides
   - `# Title` = title slide (use `<h1>`)
   - `## Heading` = slide heading (use `<h2>`)
   - `- item` = bullet list (use `<ul><li>`)
   - `**bold**` = emphasis (use `<span class="accent">` or `<span class="green">`)
   - `> quote` = blockquote (use `<blockquote class="quote">`)
   - `<img src="...">` = image (keep as-is, add `class="shadow"` if appropriate)
   - `` ```mermaid ``` `` = mermaid diagram (wrap in `<div class="mermaid-wrapper"><pre class="mermaid">`)
   - `<!-- image gallery - VERTICAL -->` = split images into separate slides (one per slide)
   - `<!-- comment -->` = other layout hints (e.g., `<!-- big quote -->`)

4. Generate HTML slides:
   - Each slide is a `<section class="slide">` (add `centered` class for title/image slides)
   - Preserve the template's head section (styles, scripts, mermaid config)
   - Preserve the navigation and script at the bottom

5. Write output to `index.html` in the same directory as the spec

6. Create standalone shareable HTML:
   - Run the build script to embed all PNG images as base64 data URIs
   - Output: `FOLDERNAME-standalone.html` (e.g., `2026-01-05-luciq-presentation-standalone.html`)
   - Creates a single self-contained file (~2-3MB) that opens in any browser

   ```bash
   {SKILLBASE}/scripts/create_standalone.sh /path/to/presentation/folder
   ```

## Slide Type Examples

### Title Slide
```html
<section class="slide centered">
  <h1>Presentation Title</h1>
  <p class="subtitle">Subtitle here</p>
</section>
```

### Bullet Points
```html
<section class="slide">
  <h2>Slide Title</h2>
  <ul>
    <li>Point with <span class="accent">emphasis</span></li>
    <li>Another point</li>
  </ul>
</section>
```

### Big Quote
```html
<section class="slide centered">
  <blockquote class="quote">
    "The quote text here"
  </blockquote>
</section>
```

### Mermaid Diagram
```html
<section class="slide centered">
  <h2>Diagram Title</h2>
  <div class="mermaid-wrapper">
    <pre class="mermaid">
graph LR
    A[Start] --> B[End]
    </pre>
  </div>
</section>
```

### Image
```html
<section class="slide centered">
  <h2>Image Title</h2>
  <img src="./image.png" alt="Description" class="shadow" />
</section>
```

### Image Gallery (2 images side by side)
```html
<section class="slide">
  <h2>Gallery Title</h2>
  <div class="columns">
    <div class="column">
      <p class="muted">Caption 1</p>
      <img src="./image1.png" alt="Image 1" />
    </div>
    <div class="column">
      <p class="muted">Caption 2</p>
      <img src="./image2.png" alt="Image 2" />
    </div>
  </div>
</section>
```

### Vertical Image Gallery (one image per slide)
When spec contains `<!-- image gallery - VERTICAL -->`, split each image into its own slide:
```html
<section class="slide centered">
  <h2>Section Title</h2>
  <p class="muted">Caption 1</p>
  <img src="./image1.png" alt="Image 1" class="shadow" style="max-height: 55vh;" />
</section>

<section class="slide centered">
  <h2>Section Title</h2>
  <p class="muted">Caption 2</p>
  <img src="./image2.png" alt="Image 2" class="shadow" style="max-height: 55vh;" />
</section>
```

## Available CSS Classes

### Slide Classes
- `.slide` - base slide
- `.slide.centered` - center content
- `.slide.top` - align to top

### Color Classes
- `.accent` - light blue (#ADD7FF)
- `.green` - mint green (#5DE4c7)
- `.yellow` - soft yellow (#fffac2)
- `.pink` - pink (#d0679d)
- `.cyan` - cyan (#89ddff)
- `.muted` - dim gray

### Layout Classes
- `.columns` - two-column grid
- `.split` - image + text side by side
- `.gallery` - flex gallery for multiple images

### Element Classes
- `.subtitle` - smaller subtitle text
- `.small` - smaller text
- `.quote` - styled blockquote
- `.big-number` - large stat/number
- `.badge` - tag/badge element
