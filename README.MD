# ku-titlepage

A LaTeX package for typesetting title pages that follow the University of
Copenhagen's official design guide (https://designguide.ku.dk/), with fully
configurable field positions, font sizes, and an optional front page image.

Originally based on [Mads Ohm Larsen's `ku-frontpage`](https://github.com/omegahm/ku-frontpage)
(2021). This version reworks the letterhead to match the current KU design
guide (as of 27-09-2026) and adds configurable positioning, font sizing, and
image sizing for every field on the page.

## Features

- Official KU letterhead (logo + faculty wordmark) for six faculties
- A fully configurable title page: assignment type, author, title, subtitle,
  supervisor, date, and an optional front page image
- Every field's **position**, **font size**, and (for the image) **size**
  can be overridden independently, without editing the package itself
- A `blank` style for a letterhead-only page with no title block
- Sets its own page size directly and scopes `\maketitle` in a local group,
  so the title page isn't shifted by `\parskip`, `geometry`, or other page
  setup already in your document's preamble

## Requirements

Requires the following packages, all commonly available in a standard LaTeX
distribution (e.g. TeX Live, MiKTeX): `eso-pic`, `graphicx`, `ifthen`,
`textpos`, `fontenc`, `tgpagella`, `tgheros`, `mathpazo`. Page size (A4) is
set directly by the package, so it does not require or load `geometry`,
and won't conflict with a `geometry` setup already in your preamble.

## Installation

Place `ku-titlepage.sty` in the same directory as your `.tex` file (or
anywhere on your local TEXINPUTS path), along with a `logos/` folder
containing the KU logo files the package expects:

- `logos/a4_logo.pdf`
- `logos/a4_text_science.pdf`
- `logos/a4_text_sund.pdf`
- `logos/a4_text_samf.pdf`
- `logos/a4_text_jura.pdf`
- `logos/a4_text_hum.pdf`
- `logos/a4_text_teo.pdf`

## Package options

Load the package with a **style** option and a **faculty** option:

```latex
\usepackage[titlepage, science]{ku-titlepage}
```

### Style options

| Option      | Description                                                       |
|-------------|--------------------------------------------------------------------|
| `titlepage` | Full title page: assignment, author, title, subtitle, supervisor, date |
| `blank`     | Letterhead only (logo + faculty wordmark), no title block         |

### Faculty options

| Option              | Faculty                          |
|----------------------|-----------------------------------|
| `science` (default) | Faculty of Science                |
| `sund`               | Faculty of Health and Medical Sciences |
| `samf`               | Faculty of Social Sciences        |
| `jura`               | Faculty of Law                    |
| `hum`                | Faculty of Humanities             |
| `teo`                | Faculty of Theology               |

## Usage

Fill in the content fields and call `\maketitle`:

```latex
\documentclass{article}
\usepackage[titlepage, science]{ku-titlepage}

\begin{document}

\thispagestyle{empty}

\assignment{PhD thesis}
\author{Svante Wold}
\title{New chemical descriptors relevant for the design of biologically active peptides}
\subtitle{A multivariate characterization of 87 amino acids}
\supervisor{Supervisor: Anders Andersen}
\date{\today}

\maketitle

\end{document}
```

With an optional front page image, using the default position and size:

```latex
\assignment{PhD thesis}
\author{Svante Wold}
\title{New chemical descriptors relevant for the design of biologically active peptides}
\subtitle{A multivariate characterization of 87 amino acids}
\supervisor{Supervisor: Anders Andersen}
\date{\today}
\frontpageimage{test-image.pdf}

\maketitle
```

## Public API reference

### Content commands

| Command                  | Argument  | Description                                  |
|---------------------------|-----------|-----------------------------------------------|
| `\assignment{text}`       | text      | Assignment type, e.g. `MSc thesis`            |
| `\author{text}`           | text      | Author (position follows assignment)                                   |
| `\title{text}`            | text      | Thesis title                                  |
| `\subtitle{text}`         | text      | Subtitle (leave empty for none)               |
| `\supervisor{text}`       | text      | Supervisor name/label                         |
| `\date{text}`             | text      | Date text, e.g. `\today`                      |
| `\frontpageimage{path}`   | file path | Optional cover image; omit to skip entirely   |

### Position commands

All take plain numbers in **mm**, `{x}{y}`, measured from the top-left
corner of the page. Call before `\maketitle`; any field left unconfigured
uses its default.

The assignment and author lines share a single textblock and position -
there is no separate `\authorposition`. The gap between the two lines is
a hardcoded `\vspace{2mm}` inside `\maketitle` in `ku-titlepage.sty`;
edit that value directly in the package if you want more or less space.

| Command                          | Default (x, y) |
|------------------------------------|-----------------|
| `\assignmentposition{x}{y}`       | 20, 195         |
| `\titleposition{x}{y}`            | 20, 217         |
| `\supervisorposition{x}{y}`       | 20, 260         |
| `\dateposition{x}{y}`             | 20, 267         |
| `\frontpageimageposition{x}{y}`   | 19, 185 (unused unless called - see above) |

### Font size commands

All take `{size}{lineheight}` with explicit units (e.g. `pt`). Call before
`\maketitle`.

| Command                              | Default (size, lineheight) |
|----------------------------------------|-------------------------------|
| `\assignmentfontsize{size}{lineheight}` | 14pt, 16pt                   |
| `\authorfontsize{size}{lineheight}`     | 14pt, 16pt                   |
| `\titlefontsize{size}{lineheight}`      | 20pt, 28pt                   |
| `\subtitlefontsize{size}{lineheight}`   | 14pt, 16pt                   |
| `\supervisorfontsize{size}{lineheight}` | 12pt, 5pt                    |
| `\datefontsize{size}{lineheight}`       | 12pt, 5pt                    |

### Front page image size

Takes `{width}{height}` - a length or dimension expression such as
`0.8\textwidth` or `120mm`.

| Command                            | Default              |
|--------------------------------------|-----------------------|
| `\frontpageimagesize{width}{height}` | `\textwidth`, `100mm` |

## Customization examples

Repositioning fields - e.g. when a long title needs more room:

```latex
\titleposition{20}{215}       % move title block up to make room
\supervisorposition{20}{270}  % push supervisor/date down to compensate
```

Resizing text and the front page image together:

```latex
\titlefontsize{26pt}{28pt}                  % larger title text
\frontpageimagesize{0.6\textwidth}{80mm}    % smaller image
\frontpageimageposition{25}{180}            % nudge it to match
```

Letterhead-only page (no title block):

```latex
\usepackage[blank, sund]{ku-titlepage}
...
\maketitle
```

## Examples

Full, ready-to-compile examples live at the repo root:

- [`example-default.tex`](example-default.tex) - a full title page with every
  position, font size, and faculty left at its package default.
- [`example-custom-layout.tex`](example-custom-layout.tex) - repositions and
  enlarges the title block, then pushes the supervisor and date positions
  down to compensate, using `\titleposition`, `\titlefontsize`,
  `\supervisorposition`, and `\dateposition`.
- [`example-frontpage-image.tex`](example-frontpage-image.tex) - adds a front
  page image with `\frontpageimage`, sized with `\frontpageimagesize`, and
  pinned to an explicit position with `\frontpageimageposition` (omit that
  call to have the image centered horizontally instead, which is the
  default).
- [`example-blank.tex`](example-blank.tex) - the `blank` style option: just
  the letterhead, no title block.

## License

Distributed under the MIT License, consistent with the original
`ku-frontpage` package it's based on.