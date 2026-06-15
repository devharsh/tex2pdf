# Third-party notices

This project redistributes and uses the following third-party components.

## busytex and TeXlyre busytex

Files: `core/texlyre-busytex.js` and everything in `core/busytex/` (`busytex.wasm`, `busytex.js`, `busytex_pipeline.js`, `busytex_worker.js`, `texlive-basic.js`, `texlive-basic.data`, and related files).

These are the busytex WebAssembly build of TeX Live and the TeXlyre busytex runner, distributed under the GNU Affero General Public License v3.0 (AGPL-3.0). Sources:

- busytex: https://github.com/busytex/busytex
- TeXlyre busytex: https://github.com/TeXlyre/texlyre-busytex

The bundled `texlive-basic.data` contains TeX Live programs and packages (TeX by Donald E. Knuth; pdfTeX, XeTeX, LuaTeX, and LaTeX packages), each under its own free-software license such as the LaTeX Project Public License, the GNU GPL, or the Knuth license.

## Bundled CTAN packages

`core/texmf/booktabs.sty`, `enumitem.sty`, and `url.sty` are from CTAN, under the LaTeX Project Public License (LPPL). `booktabs.sty` was generated from the package source. Sources: https://ctan.org/pkg/booktabs , https://ctan.org/pkg/enumitem , https://ctan.org/pkg/url

Because this project bundles AGPL-3.0 software, the combined work is licensed under AGPL-3.0. The complete corresponding source is this repository.

## PDF.js

Loaded at runtime from a CDN. Copyright Mozilla Foundation, Apache License 2.0. https://github.com/mozilla/pdf.js

## JSZip

Loaded at runtime from a CDN. Copyright Stuart Knightley, dual licensed under the MIT License or GPLv3. https://github.com/Stuk/jszip
