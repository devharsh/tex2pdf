# tex2pdf

**TeX Editor Online. Edit, compile, and view LaTeX in your browser, no software needed.**

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.20708417.svg)](https://doi.org/10.5281/zenodo.20708417)

Write, compile, and view LaTeX as a PDF, entirely in your browser. No account, no install, no server, and nothing is uploaded. Edit in an embedded VS Code editor and compile to an accessible, vector-graphics PDF, or just drop in a `.tex` file and get a PDF back. The TeX engine is bundled as WebAssembly and runs inside the page.

Live website: https://devharsh.github.io/tex2pdf/

![Screenshot of TeX Viewer Online: engine selector (XeLaTeX, pdfLaTeX), a file drop zone, live edit with VS Code controls, and the PDF preview pane](screenshots/tex2pdf.png)

## In-browser tools

Alongside the TeX viewer, the site bundles small client-side utilities. Each lives on its own page so its code loads only when you open it, keeping memory and first paint low. Everything runs in your browser; nothing is uploaded.

### TeX Editor (`index.html`)

The flagship page is a full three-panel LaTeX editor (file tree, a VS Code editor, and a live PDF preview) with two modes:

- **Editor mode** lets you create or import a project, edit `.tex` in an embedded Monaco (the VS Code editor core) with LaTeX syntax and best-practice linting, manage a file tree, and compile to PDF. It starts from accessible templates (article, report, math, beamer, CV) and publisher styles (IEEE, ACM, Springer LNCS, MDPI, Elsevier). Templates use `\DocumentMetadata` to produce tagged, accessible PDF/UA output, encourage vector graphics, and flag raster images.
- **Quick View mode** is the original simple flow: drop a single `.tex`, several files, or a `.zip`, choose XeLaTeX, pdfLaTeX, or LuaLaTeX, and preview the PDF with a one-click download.

It also connects to **GitHub and GitLab** (clone, commit, push) with a personal access token kept only in your browser, imports and exports `.zip` projects, and keeps the live VS Code folder sync. When a document needs a publisher class that is not in the bundled set, the editor fetches the class on demand from a CORS-enabled CDN mirror and recompiles; very heavy classes may still need a few extra files added manually. The best-practice linter flags inline `$...$` (suggests `\(...\)`), `eqnarray` (suggests `align`), raster `\includegraphics`, straight quotes, `\ref` (suggests `\cref`), unprefixed `\label`s, forced line breaks in body text, and a missing `\DocumentMetadata`.

### Markdown to PDF (`html/md2pdf.html`)

Render a Markdown file and save it as a PDF, entirely in the browser. It handles GitHub-flavored Markdown (tables, task lists, strikethrough), highlights code, and renders math with KaTeX. Load or drag in a `.md` file, or paste text, then Save as PDF (through the print dialog), download the HTML, copy it, or clear. It also supports live VS Code folder editing, and its libraries load on demand. A **Research paper** mode converts Markdown (with an optional YAML front matter block for title, author, abstract) to LaTeX and compiles a tagged, vector-graphics PDF with the same busytex engine as the editor, mapping headings to sections, fenced code to verbatim, tables to booktabs, images to figures, `[@key]` to `\cite`, and `$...$`/`$$...$$` to inline and display math.

### Beautify and highlight (`html/beautify.html`)

Re-indent and colour code entirely in the browser. It auto-detects the language and reformats JSON, JavaScript, CSS, HTML/XML, BibTeX, and Java, and adds syntax highlighting for Python, C, C++, and C# as well. Load a file or paste text, then copy, download, or clear the result. It also supports live VS Code folder editing.

![Beautify and highlight tool: language selector, input on the left and highlighted, re-indented output on the right](screenshots/beautify.png)

### File Compare (`html/diff.html`)

A side-by-side diff of two snippets, two text files, or two PDFs (compared by their extracted text). Toggle whitespace handling, swap sides, download the differences as a `.diff` file, or clear both sides.

![Compare tool: two inputs above a side-by-side highlighted diff of the changes](screenshots/compare.png)

### Image Compare (`html/imgcompare.html`)

Compare two images side by side, even across formats (PNG, JPG, SVG, WebP, GIF, BMP, and the first page of a PDF). For each image it reads metadata that is hard to judge by eye, dimensions, file size, megapixels, aspect ratio, colour and channels, transparency, bit depth, and DPI (from the PNG or JPEG headers), and highlights the rows that differ between the two. It also computes two similarity scores, a pixel match (RMSE, resized to a common size, so different sizes and formats can be compared) and a perceptual match (difference hash), and renders a red difference map. An **Expand** view shows both images large, side by side or top and bottom (your choice), and you exit back to the metadata and similarity details. Everything runs locally; nothing is uploaded.

![Image Compare with a PNG and a PDF side by side: previews on a transparency checkerboard, metadata tables with the differing rows highlighted, similarity scores, and a red difference map](<screenshots/png image compare.png>)

![Image Compare with two PDFs side by side: previews, metadata tables, the pixel and perceptual similarity scores, and the difference map](<screenshots/pdf image compare.png>)

### PDF Unlock (`html/pdfunlock.html`)

Unlock PDFs for editing in your browser. Keep an existing signature cryptographically valid while unlocking form fields with an incremental update, or make the file fully editable by flattening or removing the signature and stripping the AcroForm AppendOnly `SigFlags`, certification (DocMDP) `Perms`, XFA, and read-only flags. It also removes password or permission encryption with qpdf compiled to WebAssembly. It previews the result with PDF.js and supports live VS Code folder watching. Nothing is uploaded.

### CyberChef (`html/cyberchef.html`)

The bundled Cyber Swiss Army Knife (by GCHQ) for encoding, encryption, compression, data formats, and hundreds of other operations, running locally with no server. Its modules load on demand when you open the tool.

![CyberChef running locally: operations list, recipe area, and input and output panels](screenshots/cyberchef.png)

## Why

Most LaTeX setups need a local TeX install or a cloud service that uploads your files. tex2pdf does neither. A TeX engine (busytex) plus a TeX Live package set are shipped as static files and run client side, so compilation is private and self contained.

## How it compares

TeX Editor Online fills a specific niche: a free, private, no-account, open-source editor that compiles in your browser. It deliberately trades cloud features (AI, real-time collaboration) and full TeX Live coverage for privacy and zero setup. This is an honest comparison; pricing is approximate as of 2026 and changes often, so check each tool's site.

| Capability | TeX Editor Online | Overleaf | Bibby AI | VS Code + LaTeX Workshop | TeXStudio | Papeeria |
| --- | --- | --- | --- | --- | --- | --- |
| Runs in the browser, no install | Yes | Yes | Yes | No (desktop) | No (desktop) | Yes |
| No account required | Yes | No | No | Yes | Yes | No |
| Compiles locally; nothing uploaded | Yes | No (cloud) | No (cloud) | Yes (local) | Yes (local) | No (cloud) |
| Open source | Yes (AGPL-3.0) | Partly (self-host Community Edition) | No | Yes (Code-OSS + extension) | Yes (GPL) | No |
| Price | Free | Free tier + paid | Free tier + paid | Free | Free | Free tier + paid |
| Git (GitHub / GitLab) | Yes, in browser | Yes (paid) | Not advertised | Yes | External tool | Yes |
| Real-time collaboration | No (use Git) | Yes | Planned | Via Live Share | No | Yes |
| AI assistance | No | No | Yes | Via extensions | No | No |
| Full TeX Live / heavy classes (e.g. acmart, large theses) | Limited (basic set plus bundled extras) | Yes | Yes | Yes (local) | Yes (local) | Yes |
| Telemetry / tracking | None | Yes | Yes | Optional telemetry | None | Yes |

Paid plans, as a rough 2026 guide: Overleaf premium starts around $7 to $13 per month depending on billing, and Papeeria private projects start around $5 per month; verify current numbers on each vendor's pricing page.

In short: choose TeX Editor Online when privacy, no sign-up, and zero install matter most and your document uses mainstream packages; choose Overleaf, Bibby AI, or Papeeria for cloud collaboration, AI help, or heavy publisher classes; choose VS Code or TeXStudio for a full local toolchain.

## Features

- A three-panel editor: file tree, a Monaco (VS Code) editor with LaTeX syntax, and a live PDF preview.
- Editor mode and a simple Quick View mode for just turning a `.tex` into a PDF.
- Three engines: XeLaTeX (default), pdfLaTeX, and LuaLaTeX.
- Best-practice linter that enforces semantic markup (math mode, `align`, `\cref`, label prefixes, vector graphics, no manual line breaks).
- Accessible, tagged PDF output via `\DocumentMetadata` (PDF/UA), with raster-image warnings.
- Templates for articles, reports, math, slides, CVs, and publisher styles (IEEE, ACM, Springer, MDPI, Elsevier).
- GitHub and GitLab integration (clone, commit, push) with the token kept only in your browser.
- Import and export `.zip` projects; upload several files together (figures, `.bib`, `.cls`).
- The basic TeX Live set plus bundled extras (booktabs, enumitem, url), with on-demand fetch of missing classes.
- Automatic main-file detection, an elapsed timer and progress bar, inline preview, and one-click PDF download.
- BibTeX runs automatically when a `.bib` file is present.
- Live VS Code folder sync. Responsive layout and keyboard-accessible controls. Custom 404 page.

## Choosing an engine

Keep the default, **XeLaTeX**. In this in-browser engine it handles fonts most reliably because it uses vector fonts directly.

Use **pdfLaTeX** only if you prefer it. It is slightly faster, but it can fail when a document needs a bitmap font it must build on the fly, which is not possible in WebAssembly (you will see a font or `mktexpk` error in the log). If that happens, switch back to XeLaTeX and compile again.

## Using it

1. Open the site.
2. Leave the engine on XeLaTeX (or pick pdfLaTeX).
3. Drag in your `.tex` file, multiple files, or a `.zip` of your project.
4. If there is more than one `.tex`, pick the main one (the file with `\documentclass`).
5. Click Compile to PDF. The preview appears on the right; use Download PDF to save it.

The first compile loads the engine and TeX Live (about 125 MB). Your browser caches it, so later compiles are fast (a few seconds).

## Live editing with VS Code

You can keep your usual editor and have the output rebuild as you save, with no upload:

1. Click **Open a folder** and choose your project folder (grant read access once).
2. Click **Open VS Code**, then in vscode.dev choose File, Open Folder, and pick the same folder.
3. Edit and save in VS Code. The tool watches the folder and rebuilds automatically on each save (toggle with the auto-run checkbox).

This uses the browser File System Access API and works in Chrome and Edge. In other browsers the button is disabled and you can still upload files manually. The TeX to PDF, Markdown to PDF, and Beautify pages all support this workflow.

## How it works

Your files are read with the browser File API and written into the engine's in-memory filesystem. busytex, a WebAssembly build of TeX Live, compiles them in a Web Worker. The bundled TeX Live "basic" set is served as static files; a few common packages that are not in basic live in `core/texmf/` and are written into the project at compile time. The resulting PDF bytes are rendered with PDF.js. Nothing is uploaded or stored.

## Package coverage

The bundled basic tier already includes the LaTeX kernel, article/report/book classes, amsmath, amssymb, geometry, graphics, hyperref and its dependencies, and many more. The full TeX Live (the "extra" tier, ~340 MB and ~27k files) does not load reliably in a browser, so instead a small set of extra packages is bundled in `core/texmf/`:

```
core/texmf/booktabs.sty
core/texmf/enumitem.sty
core/texmf/url.sty
core/texmf/manifest.json   lists the bundled files
```

To add another package, drop its `.sty` (and any dependencies not already in basic) into `core/texmf/`, add the filename to `manifest.json`, and commit. The app writes every bundled file into the compile directory, so `\usepackage{...}` finds it. A document that needs a package not in basic and not bundled will fail with a "File not found" message in the log naming the missing file.

## Run locally

Opening `index.html` from a `file://` path will not work, because browsers block Web Workers, WebAssembly, and ES module loading there. Serve the folder over HTTP instead:

```bash
git clone https://github.com/devharsh/tex2pdf.git
cd tex2pdf
python3 -m http.server 8000
```

Open `http://localhost:8000/` and compile `sample.tex`.

## Deploy your own

This is a static site, so any static host works. For GitHub Pages:

1. Fork this repository, or create your own and push these files (everything under `core/` must be committed; they are normal files, not Git LFS).
2. In the repository, go to Settings, then Pages.
3. Under Build and deployment, set Source to Deploy from a branch, choose `main` and the `/ (root)` folder, and save.
4. Your site goes live at `https://USERNAME.github.io/tex2pdf/`.

The included `.nojekyll` keeps GitHub Pages from processing the site with Jekyll. The app computes its asset path automatically, so it works under any subpath. If you host under a different name, custom domain, or path, update the single link in `404.html`.

## Project structure

```
index.html                 The TeX editor UI (home page; at root for GitHub Pages)
404.html                   Custom not-found page (at root for GitHub Pages)
sitemap.xml, robots.txt    Search-engine sitemap and crawl rules
html/md2pdf.html           Markdown to PDF and research-paper tool
html/beautify.html         Beautify and highlight tool
html/diff.html             Compare tool
html/imgcompare.html       Image Compare tool
html/pdfunlock.html        PDF Unlock tool
html/cyberchef.html        CyberChef wrapper
screenshots/               Demo images used in this README
sample.tex                 Example document for testing
core/texlyre-busytex.js    The busytex runner (ES module)
core/busytex/              Engine and TeX Live basic bundle (busytex.wasm, busytex.js, workers, texlive-basic.*)
core/texmf/                Extra packages not in the basic set (booktabs, enumitem, url)
cyberchef/                 Bundled CyberChef (index.html + assets + modules)
.nojekyll                  Serve files as-is on GitHub Pages
NOTICE.md                  Third-party licenses and credits
```

The home page (`index.html`) and `404.html` stay at the repo root because GitHub Pages serves those from the root; the other tool pages live in `html/`.

## Limitations

- First load is about 125 MB (then cached). It is heavier on phones than on desktops.
- pdfLaTeX cannot build missing bitmap fonts; use XeLaTeX (the default) for those documents.
- Coverage is the basic tier plus the bundled extras; arbitrary CTAN packages are not all available (see Package coverage).
- Each visitor downloads the assets once. On GitHub Pages the soft bandwidth limit is about 100 GB per month, roughly 800 first-time loads.
- Very heavy publisher classes, in particular ACM `acmart`, are not supported in-browser. They pull in dozens of packages plus commercial fonts (Libertine, newtx, Inconsolata, unicode-math) and TikZ, and the WebAssembly engine cannot reliably register those fonts. Compile those documents with a full TeX (a local TeX Live or Overleaf); the editor is still useful for writing and managing the source. Lighter classes such as IEEEtran are realistic in-browser.

## Privacy

All processing happens in your browser. The only network requests are loading the page, the engine and package files from this site, and helper libraries from a CDN (PDF.js and JSZip for the TeX viewer, on the tool pages marked, DOMPurify, highlight.js, and KaTeX, and pdf-lib and qpdf-wasm for PDF Unlock). Image Compare analyses pixels with the built-in canvas. Your document content is never transmitted.

## Credits and licenses

- TeX engine and WebAssembly build: [busytex](https://github.com/busytex/busytex) and the [TeXlyre busytex](https://github.com/TeXlyre/texlyre-busytex) distribution, under AGPL-3.0.
- PDF rendering: [PDF.js](https://github.com/mozilla/pdf.js) by Mozilla, Apache-2.0.
- Zip reading: [JSZip](https://github.com/Stuk/jszip), MIT.
- In-browser editor: [Monaco Editor](https://github.com/microsoft/monaco-editor) by Microsoft, MIT.
- Git in the browser: [isomorphic-git](https://github.com/isomorphic-git/isomorphic-git) and [LightningFS](https://github.com/isomorphic-git/lightning-fs), MIT.
- Markdown rendering: [marked](https://github.com/markedjs/marked) (MIT), [DOMPurify](https://github.com/cure53/DOMPurify) (Apache-2.0 or MPL-2.0), [highlight.js](https://github.com/highlightjs/highlight.js) (BSD-3-Clause), and [KaTeX](https://github.com/KaTeX/KaTeX) (MIT).
- PDF Unlock: [pdf-lib](https://github.com/Hopding/pdf-lib) (MIT) and [qpdf](https://github.com/qpdf/qpdf) compiled to WebAssembly (Apache-2.0).
- Bundled LaTeX packages in `core/texmf/` (booktabs, enumitem, url, and any added for specific documents such as wrapfig, footmisc, xcolor, listings, biblatex, and others) are unmodified redistributions from CTAN and TeX Live, each under its own license, most commonly the LaTeX Project Public License (LPPL). See [NOTICE.md](NOTICE.md).

## Author

Devharsh Trivedi, PhD, CISSP. ORCID: https://orcid.org/0000-0001-6374-7249

## Citation

If you use this project, please cite it. Citation metadata is in [CITATION.cff](CITATION.cff). A plain-text form:

> Trivedi, D. (2026). tex2pdf: TeX Viewer Online (v1.0.0) [Software]. Zenodo. https://doi.org/10.5281/zenodo.20708417

## License

Licensed under the GNU Affero General Public License v3.0 or later (AGPL-3.0-or-later); see [LICENSE](LICENSE). Because this project bundles busytex, which is AGPL-3.0, the combined work is covered by the AGPL-3.0. Third-party notices are in [NOTICE.md](NOTICE.md).
