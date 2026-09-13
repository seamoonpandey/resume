# Seamoon Pandey — Résumé & CV

LaTeX sources for two documents with different audiences.

| Document | Audience | Output |
|---|---|---|
| [`cv.tex`](cv.tex) | **Master's admissions** (Germany / EU) | [`cv.pdf`](cv.pdf) — 1 page, A4 |
| [`resume.tex`](resume.tex) | **Industry / job applications** | [`resume.pdf`](resume.pdf) — 1 page, A4 |

Live links: [GitHub](https://github.com/seamoonpandey) · [LinkedIn](https://www.linkedin.com/in/seamoonpandey/)

## Which one to send

**`cv.pdf`** leads with Education and Language Proficiency, because that is what an
admissions committee reads first. It states the aggregate (67.98%, First Division)
alongside the Tribhuvan University grading scale, so a reader outside Nepal can place
the figure — a mark in the high sixties here is not what the same number means under a
more generous system. The capstone is framed as a *thesis*, and the leadership and
freelance work sit below the academic material.

**`resume.pdf`** leads with the Summary and Flagship Project, because a hiring manager
reads for capability first. Education sits at the bottom.

Both are single-column for clean parsing by applicant-tracking systems and by the
automated document handling used in university admissions.

## Build

```bash
pdflatex -interaction=nonstopmode cv.tex
pdflatex -interaction=nonstopmode resume.tex
```

No second pass is needed — neither document uses cross-references.

### Alternatives

- **Tectonic** (self-contained, fetches packages on demand): `tectonic cv.tex`
- **Overleaf:** upload the `.tex` and compile with pdfLaTeX.

## Fonts — read before changing

Both documents use **Nimbus Sans** via `\usepackage[scaled=0.92]{helvet}`.

They previously declared `\usepackage{lato}`. That was silently broken: MiKTeX
installs `lato.sty` and `lato.map` but **not** the Lato TFM/Type 1 font files, so
LaTeX fell back to Computer Modern and rendered the body text as `.pk` **bitmaps**
(Type 3 fonts). The committed PDF was never actually set in Lato.

Type 3 bitmaps matter here beyond looks — they print poorly and extract badly, and
university application portals parse the text of submitted PDFs.

Verify after any font change:

```bash
pdffonts cv.pdf
```

Every row should read **Type 1** (or Type 0/TrueType). If you see **Type 3**, the font
did not load and you are shipping bitmaps.

To use Lato on a distribution that has the real font files (Overleaf, TeX Live full),
swap the `helvet` lines for `\usepackage{lato}`.

## Layout notes

- Section headings use `titlesec`. The heading format is:

  ```latex
  \titleformat{\section}{\large\bfseries}{}{0em}{\MakeUppercase}[...\color{rulegray}\hrule...]
  ```

  `\MakeUppercase` belongs in the *before-code* (4th) argument. It was previously
  `\uppercase` in the *format* (2nd) argument, which swallowed the trailing rule code
  and raised `Undefined color 'RULEGRAY'` six times per build — the section rules were
  drawing black instead of gray.

- Tuned to fit one A4 page each. If content is added, reclaim space via `\parskip`,
  the `\titlespacing` of `\section`, or the `geometry` margin (`0.5in` in `cv.tex`,
  `0.6in` in `resume.tex`) before cutting content.

## Files

| File | Purpose |
|------|---------|
| `cv.tex` | Academic CV source — edit this for admissions |
| `resume.tex` | Industry résumé source |
| `cv.pdf` / `resume.pdf` | Compiled output (tracked) |
| `README.md` | This file |
| `.gitignore` | Ignores LaTeX build artifacts |
