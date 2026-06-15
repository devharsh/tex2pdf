# tex2pdf: in-browser LaTeX to PDF

A static web app that lets you upload LaTeX files and view them as a PDF, entirely in the browser. The LaTeX compilation runs client side via a WebAssembly build of pdfTeX and XeTeX (SwiftLaTeX), and the resulting PDF is rendered with PDF.js. There is no backend. Uploaded files are processed in memory and are never sent to or stored on any server.

## Features

- Upload a single `.tex` file, multiple files (images, `.bib`, `.cls`), or a `.zip` project.
- Choose the engine: pdfTeX (fast, standard documents) or XeTeX (full Unicode and system fonts).
- Automatic main-file detection, with a picker when several `.tex` files are present.
- Inline PDF preview (all pages) plus a download button.
- Custom 404 page.

## How it works

1. Your files are read in the browser with the File API and written into the engine's in-memory filesystem.
2. SwiftLaTeX (pdfTeX or XeTeX compiled to WebAssembly) compiles them locally in a Web Worker.
3. Any LaTeX packages the document needs are fetched on demand from the SwiftLaTeX TeX Live mirror at `https://texlive2.swiftlatex.com`.
4. The PDF bytes are handed to PDF.js and drawn onto canvases. Nothing is uploaded or persisted.

## Files in this repository

- `index.html`: the whole application (HTML, CSS, and JS in one file).
- `404.html`: custom not-found page.
- `PdfTeXEngine.js`, `XeTeXEngine.js`: SwiftLaTeX engine wrappers.
- `swiftlatexpdftex.js` / `.wasm`, `swiftlatexxetex.js` / `.wasm`: the compiled engines and their Web Worker glue. These must sit next to `index.html` because the engine loads its worker by a relative path.
- `.nojekyll`: tells GitHub Pages to serve files as-is (no Jekyll processing).
- `sample.tex`: a small document for testing.

## Test locally first (optional)

Opening `index.html` directly with a `file://` URL will not work, because the LaTeX engine runs in a Web Worker and loads WebAssembly, which browsers block on `file://`. Serve the folder over HTTP instead:

```bash
cd tex2pdf
python3 -m http.server 8000
```

Then open `http://localhost:8000/` and compile `sample.tex`.

## Deploy to GitHub Pages

From inside this `tex2pdf` folder:

```bash
git init
git add .
git commit -m "Static in-browser LaTeX to PDF viewer"
git branch -M main
git remote add origin https://github.com/USERNAME/tex2pdf.git   # create this empty repo on github.com first
git push -u origin main
```

Then enable Pages:

1. On GitHub, open the repository, go to Settings, then Pages.
2. Under Build and deployment, set Source to Deploy from a branch.
3. Select branch `main` and folder `/ (root)`, then Save.
4. After a minute, your site is live at `https://USERNAME.github.io/tex2pdf/`.

Open that URL, drop in `sample.tex`, and click Compile to PDF.

Note on the 404 link: `404.html` links back to `/tex2pdf/`. If you name the repository something other than `tex2pdf`, edit that link. If you use a custom domain or a user/organization Pages site served from the root, change the link to `/`.

## Caveats

- The first compile downloads the engine (about 2 MB for pdfTeX, about 3 MB for XeTeX) plus any required packages, so it can be slow. Later compiles are much faster.
- It is heavier on phones than on desktops.
- Very complex documents, exotic packages, or multi-pass BibTeX or biber workflows may hit limits.
- Compilation depends on the SwiftLaTeX TeX Live mirror being reachable. If a compile hangs or fails to find packages, the mirror may be temporarily down.

## Privacy

All processing is local to your browser. The only network calls are to load the page assets, the PDF.js and JSZip libraries from a CDN, and LaTeX packages from the SwiftLaTeX mirror during compilation. Your document content is not transmitted.

## Licenses and credits

- SwiftLaTeX engine and WebAssembly builds: copyright Elliott Wen and contributors, released under EPL-2.0 or GPL-2.0 with Classpath exception. Project: https://github.com/SwiftLaTeX/SwiftLaTeX
- PDF.js: Mozilla, Apache-2.0.
- JSZip: MIT.

The underlying TeX and LaTeX programs are distributed under their own respective licenses. See `NOTICE.md` for details.
