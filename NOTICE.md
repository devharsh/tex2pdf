# Third-party notices

This project redistributes and uses the following third-party components.

## busytex and TeXlyre busytex

Files: `core/texlyre-busytex.js` and everything in `core/busytex/` (`busytex.wasm`, `busytex.js`, `busytex_pipeline.js`, `busytex_worker.js`, `texlive-basic.js`, `texlive-basic.data`, and related files).

These are the busytex WebAssembly build of TeX Live and the TeXlyre busytex runner, distributed under the GNU Affero General Public License v3.0 (AGPL-3.0). Sources:

- busytex: https://github.com/busytex/busytex
- TeXlyre busytex: https://github.com/TeXlyre/texlyre-busytex

The bundled `texlive-basic.data` contains TeX Live programs and packages (TeX by Donald E. Knuth; pdfTeX, XeTeX, LuaTeX, and LaTeX packages), each under its own free-software license such as the LaTeX Project Public License, the GNU GPL, or the Knuth license.

## Bundled CTAN packages

`core/texmf/` bundles LaTeX support files taken from CTAN and the TeX Live distribution so that documents compile in the browser without a network round trip. These include `booktabs.sty`, `enumitem.sty`, and `url.sty`, and may include additional packages added for specific documents (for example `wrapfig`, `footmisc`, `xcolor`, `listings`, `array`/`tools`, `multirow`, `ragged2e`, `setspace`, `titlesec`, `lipsum`, `hyphenat`, and `biblatex` with its style and localization files). Each bundled file remains under its own license, most commonly the LaTeX Project Public License (LPPL); a few carry other free-software licenses (for example the GNU GPL). They are unmodified redistributions of the corresponding CTAN / TeX Live packages. Sources: https://ctan.org and https://tug.org/texlive/

Because this project bundles AGPL-3.0 software, the combined work is licensed under AGPL-3.0. The complete corresponding source is this repository.

## PDF.js

Loaded at runtime from a CDN. Copyright Mozilla Foundation, Apache License 2.0. https://github.com/mozilla/pdf.js

## JSZip

Loaded at runtime from a CDN. Copyright Stuart Knightley, dual licensed under the MIT License or GPLv3. https://github.com/Stuk/jszip

## Monaco Editor

Loaded at runtime from a CDN and used as the in-browser code editor on the editor page. Copyright Microsoft, MIT License. https://github.com/microsoft/monaco-editor

## isomorphic-git and LightningFS

Loaded at runtime from a CDN for in-browser GitHub and GitLab access (clone, commit, push) on the editor page. MIT License. https://github.com/isomorphic-git/isomorphic-git and https://github.com/isomorphic-git/lightning-fs

## On-demand classes

The editor can fetch publisher document classes (for example IEEEtran, acmart, llncs, mdpi, elsarticle) on demand from CORS-enabled CDN mirrors of their upstream repositories. Those files remain under their own licenses, typically the LaTeX Project Public License (LPPL). They are fetched into the browser at compile time and are not redistributed in this repository.
