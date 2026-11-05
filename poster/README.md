# NeurIPS poster (editable)

Landscape **36 × 24 in** poster sourced from the project page (`../site/`).

## Overleaf

1. New project → Upload Project (zip the `poster/` folder), **or** blank project and upload `neurips_poster.tex` plus the `figures/` folder.
2. Set the main document to `neurips_poster.tex`.
3. Compiler: **pdfLaTeX**.
4. Recompile.

Do **not** add `\usepackage{hyperref}` or `\usepackage{geometry}` — beamer/beamerposter already load them (that clash was a common Overleaf failure mode).

## Local build

```bash
cd poster
pdflatex neurips_poster.tex
```

Output: `neurips_poster.pdf`. Optional `qrcode` package enables the project-page QR; without it a text fallback is used.

### Size

Default is 36″ × 24″ (`width=91.44`, `height=60.96` cm). For A0 portrait:

```latex
\usepackage[orientation=portrait,size=a0,scale=0.88]{beamerposter}
```

## Edit tips

- Colors match the site (`Accent` teal, `Signal` orange).
- Numbers track website Tables 12, 4/16, and 6 (Figs. 4–6, 8).
- Keep `[fragile]` on the frame if you edit complex content inside it.
