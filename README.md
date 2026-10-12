# PDF Compressor

A free PDF compressor from **Tools By JRN**. It runs completely in the browser, so your file is never uploaded anywhere.

## Features

- Compress PDFs up to **50 MB**
- Three presets: **Light** (best quality), **Medium** (recommended), **Strong** (smallest file)
- **Size limit mode**: type the maximum size a website allows (for example 2 MB or 500 KB) and the tool picks the best quality that fits
- **Size preview before compressing**: shows the current size next to the estimated size, and an estimate on each preset button
- Real before and after sizes shown after compressing
- Drag and drop or click to choose a file
- Works on desktop and mobile
- Single static HTML file, no build step, no backend

## How it works

1. The PDF is opened with [pdf.js](https://mozilla.github.io/pdf.js/).
2. Each page is drawn onto a canvas at a chosen resolution (dpi) and saved as a JPEG at a chosen quality.
3. The JPEG pages are put back together into a new PDF with [jsPDF](https://github.com/parallax/jsPDF).

The quality ladder, from best to smallest:

| Step | Resolution | JPEG quality |
|------|-----------|--------------|
| 1 | 150 dpi | 0.80 |
| 2 | 130 dpi | 0.70 |
| 3 | 110 dpi | 0.60 |
| 4 | 96 dpi | 0.55 |
| 5 | 85 dpi | 0.50 |
| 6 | 72 dpi | 0.45 |
| 7 | 60 dpi | 0.40 |
| 8 | 50 dpi | 0.35 |
| 9 | 40 dpi | 0.30 |

The presets use steps 1 (Light), 3 (Medium) and 6 (Strong).

### Size estimates

When a file is chosen, up to three pages are sampled and compressed at every step. The result is scaled up by the page count to estimate the final size. Estimates are close but not exact, which is why the real size is shown after compressing.

### Size limit mode

1. Pick the best step whose estimate is under 92% of the target.
2. Compress the whole document with that step.
3. If the real result is still over the target, move to a smaller step and try again (up to 5 attempts).
4. If even the smallest step is too big, say so and suggest splitting the PDF.

## Limitations

- Pages become images, so text in the compressed PDF **cannot be selected or searched**. Keep your original if you need that.
- Password-protected or damaged PDFs cannot be compressed.
- Very strict limits on long documents may not be reachable (for example 500 KB for 100 pages).
- Large files with many pages take longer, especially on phones.
- Already optimized PDFs may come out the same size or larger. The tool warns you when that happens.

## Settings you can change

All settings are at the top of the `<script>` section in `pdf-compressor.html`:

| Setting | What it does |
|---------|--------------|
| `MAX_SIZE` | Maximum upload size (currently 50 MB) |
| `STEPS` | The quality ladder (dpi and JPEG quality for each step) |
| `PRESETS` | Which ladder step Light, Medium and Strong use |
| `SAFETY` | How far under the target to aim in size limit mode (currently 0.92) |

If you change `MAX_SIZE`, also update the "PDF files up to 50 MB" text in the drop zone and the error message in `setFile`.


## Tech

- Plain HTML, CSS and JavaScript
- pdf.js 3.11.174 and jsPDF 2.5.1, loaded from cdnjs

## Credits

Made by Jony Roy as part of [Tools By JRN](https://toolsbyjrn.vercel.app).
