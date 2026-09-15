# PDF Compressor — Single File

Compress PDF files in the browser. The application is one self-contained HTML file with no installation, no build step, no server and no network requests. Document data is never transmitted; parsing, image re-encoding and file assembly all happen in local browser memory.

The engine is built so that a page, an image or a character of text cannot be lost. Every rebuilt image is decoded again **out of the finished file** and compared with the original before that file is handed back.

---

## Contents

| File | Purpose |
| --- | --- |
| `index.html` | The complete application: markup, styles, logo, compression engine and PDF library |
| `README.md` | Documentation |
| `LICENSE` | MIT license |

---

## Integrity model

Nothing is ever deleted. An embedded raster image may be **replaced**, and only by a candidate that survives four independent checks.

**1. Structural dependency guard.** Some image streams are load-bearing: another object dictates their colour space, bit depth and sample meaning. Re-encoding them produces a technically valid file that viewers render incorrectly, which is how an image disappears while still being present in the document. The engine walks the whole object graph, including direct dictionaries and nested Form XObjects, and excludes every stream reachable as:

- an `/SMask` soft mask,
- an explicit `/Mask` stencil or colour-key mask,
- an `/Alternates` entry,
- any XObject inside an `ExtGState` `/SMask` luminosity or alpha group, at any nesting depth.

**2. Encoding exclusions.** Streams whose sample values would not survive a JPEG round-trip are never re-encoded: `/Decode` remapping arrays, `/SMaskInData` alpha, indexed JPEG, CMYK and other four-component spaces, and the `JPX`, `JBIG2`, `CCITT`, `RunLength` and `LZW` filters.

**3. Candidate gate.** Each replacement is decoded back from its own encoded bytes and compared with the source using a 32 × 32 perceptual signature. It is rejected — leaving the original in place — if it fails to decode, decodes at the wrong dimensions, diverges beyond a fixed threshold, collapses into a flat or blank image, or is not meaningfully smaller.

**4. Post-save audit.** The finished file is re-parsed from its own bytes and compared with a snapshot of the original. The audit checks page count, every image object by reference, font count, annotation count, the role and masking relationships of every XObject, and a checksum of every page's decoded content stream. Then **every replaced image is extracted from the saved file, decoded again, and compared with the original pixels.** A single mismatch discards the result.

**5. Fallback.** The engine returns the smallest candidate that passes the audit, choosing between the optimised file, a lossless rebuild, and the untouched original. The original is always the final fallback, so the output is never larger than the input and never unverified.

Page content streams, text, fonts, vector graphics, links, annotations and form fields are never rewritten.

---

## Compression levels

| Level | Behaviour |
| --- | --- |
| **High** | Images resampled to 1200 px on the long edge at quality 0.62 |
| **Medium** | Images resampled to 1700 px at quality 0.78 |
| **Low** | Images resampled to 2400 px at quality 0.90 |
| **Lossless** | No image is resampled or re-encoded; every pixel is identical to the original |

All four levels also run a **byte-exact stream optimisation** pass. Each eligible image stream is re-deflated, with PNG `Up` and `Paeth` predictors evaluated and the smallest encoding selected. Because the decoded samples do not change, this is safe for every colour space, including CMYK, indexed palettes and masks. Each candidate is inflated again and compared byte for byte before acceptance. This is what allows the Lossless level to reduce file size, and it recovers savings on images the lossy pass declines to touch.

Estimated sizes for all four levels are computed from the document's actual image data and shown before compression runs. Estimates are deliberately conservative.

---

## Features

- Entirely client-side; no upload, no telemetry, no analytics, no cookies
- No external dependencies: no CDN links, no external images, no fonts to download
- Runs from a local disk over `file://` as well as from a web server
- Four compression levels with pre-computed size estimates
- Byte-exact lossless stream optimisation on every level
- Per-result integrity report naming what was verified
- Output is never larger than the input
- Searchable text, fonts, links, annotations and form fields preserved
- Drag and drop or file picker, keyboard operable throughout
- Light and dark themes, persisted between sessions
- English and Arabic interfaces with full right-to-left layout
- Automatic download on completion, with a manual re-download control

---

## Usage

1. Open `index.html` in a modern browser, or visit the published URL.
2. Select a PDF, or drag one onto the drop area.
3. Review the estimated size for each level.
4. Choose a level and select **Compress PDF**. The result downloads automatically.
5. Read the integrity line beneath the result to confirm what was verified.

---

## Publishing to GitHub Pages

1. Upload `index.html`, `README.md` and `LICENSE` to the repository.
2. Open **Settings → Pages**.
3. Under **Source**, select **Deploy from a branch**.
4. Choose branch `main` and folder `/ (root)`, then select **Save**.
5. Reload the Pages settings screen after one to two minutes to see the published URL.

The application is a single static file, so it can also be served from any web server or intranet share, or opened directly from disk.

---

## Browser support

Requires a current version of Chrome, Edge, Firefox or Safari. The engine uses `createImageBitmap`, canvas image encoding, `CompressionStream`, `DecompressionStream` and `Blob`.

---

## Limitations

- Password-protected PDFs cannot be opened.
- Documents consisting only of text and vector graphics have little to compress.
- Soft masks, stencil masks, CMYK and JPEG 2000 images are never re-encoded; they benefit only from lossless stream optimisation.
- Very large documents are bounded by available browser memory; roughly 100 MB or 300 pages is a practical guide.

---

## Testing

The engine is validated against a twenty-file corpus modelled on real generator output: photographic and JPEG-encoded soft masks, explicit stencil masks, colour-key masks, luminosity transparency groups, images nested in Form XObjects, one image shared across pages, ICCBased RGB, optional-content groups, rotated pages, full-page scans without text, Flate with PNG `Up` and `Paeth` predictors, indexed palettes, one-bit bitonal with `/Decode` inversion, sixteen-bit RGB, CMYK in both Flate and DCT form, JPEG with `/Decode` inversion, and mixed multi-page documents.

Every file is compressed at all four levels — eighty runs — and each output is audited with two independent PDF libraries for page count, page size, rotation, per-page extracted text, per-page rendered pixels, and a decoded pixel comparison of **every image object**, including images nested inside Form XObjects and images used as masks. Soft-mask colour space and attachment are asserted separately, because that is the failure mode that makes an image vanish in a viewer while remaining present in the file.

A separate robustness suite covers empty pages, thirty-page blank documents, truncated files, random bytes, zero-length input and encrypted documents. Malformed input must fail cleanly with a message; valid input must never lose data or grow.

---

## Contributing

1. Fork the repository and create a branch from `main`.
2. Make changes in `index.html`.
3. Verify in at least one Chromium-based and one non-Chromium browser: file intake, estimation, all four levels, both themes, both languages, output text selectability, and the integrity report.
4. Confirm that no change lets an image reachable as a mask or inside a soft-mask group enter the lossy path, and that no change bypasses the post-save audit.
5. Open a pull request describing the change and the verification performed.

---

## License

Released under the MIT License. See `LICENSE`. Bundled third-party library code retains its original license notices, preserved inside `index.html`.

---

## Keywords

pdf compressor, compress pdf, compress pdf online, reduce pdf file size, pdf size reducer, shrink pdf, optimize pdf, pdf optimizer, lossless pdf compression, offline pdf compressor, browser pdf compressor, client side pdf compression, compress pdf without uploading, private pdf compressor, secure pdf compression, free pdf compression tool, javascript pdf compressor, single file web app, static site pdf tool, github pages pdf compressor, open source pdf compressor
