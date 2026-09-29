# Kilambi Research LaTeX templates

Single source of truth for every paper's LaTeX setup. Three files:

- `kr-common.tex` — shared preamble. Holds `\documentclass[11pt]{article}`,
  all packages, the title-page macro, and the evidence-grade macro.
- `kr-trackA.tex` — Track A (scientific/mathematical/physics, journal-article
  structure). Inputs `kr-common`, adds the Track A title-page wrapper.
- `kr-trackB.tex` — Track B (economics/market research, NBER working-paper
  structure). Inputs `kr-common`, adds the JEL macro and the Track B
  title-page wrapper.

## Which file to input

A paper's `.tex` file must begin with exactly one of these as its first line
(the template supplies `\documentclass`, so nothing may precede it):

```latex
\input{kr-trackA}   % Track A papers
\input{kr-trackB}   % Track B papers
```

Never copy the preamble into a paper. If a paper needs something the template
lacks, add it to the template, not the paper.

## Inclusion mechanism: TEXINPUTS

Papers live in scattered directories, so the templates are found through the
`TEXINPUTS` environment variable, not relative paths. Set it once per shell
(the trailing colon keeps the default TeX search path intact):

```sh
export TEXINPUTS="$HOME/workspace/research-site/templates:"
```

Then compile from the paper's own directory as usual. The same variable lets
`kr-trackA.tex` / `kr-trackB.tex` find `kr-common.tex` via their nested
`\input`.

## Macro signatures

`\krtitlepage{number}{title}{program}{date}{contactemail}{abstract}{keywords}{jel}`
Core title-page macro (defined in `kr-common.tex`). Prints the paper-number
line ("Kilambi Research Working Paper No. KR-WP-2026-0X"), title, the
"Kilambi Research" byline, the desk line ("Kilambi Research -- \<Program\>
Program"), contact email, date, abstract block, keywords line, and a JEL line
only when the last argument is non-empty. No disclaimer footnote, no funding
line: both are excluded by house rule. The title page is unnumbered; body
pages continue from page 2.

`\krtitlepageA{number}{title}{program}{date}{contactemail}{abstract}{keywords}`
Track A wrapper (defined in `kr-trackA.tex`). Drops the JEL slot; Track A
papers carry no JEL classification.

`\krjel{codes}` — Track B only (defined in `kr-trackB.tex`). Stores the JEL
classification codes, e.g. `\krjel{E43, G12}` in the preamble.

`\krtitlepageB{number}{title}{program}{date}{contactemail}{abstract}{keywords}`
Track B wrapper (defined in `kr-trackB.tex`). Takes the JEL line from `\krjel`.

`\evgrade{Level}{Grade}` — unified evidence vocabulary (defined in
`kr-common.tex`). Level is Observed, Reconstructed, Modelled, or Assumed;
Grade is A, B, or C. Renders as small caps, e.g. `\evgrade{Observed}{A}`
prints (Observed, A).

## Bibliography style

House style is Chicago author-date via natbib (`authoryear,round` options).
`chicago.bst` is not installed in this TeX Live distribution (verified with
`kpsewhich chicago.bst`), so the template sets `\bibliographystyle{plainnat}`:
the natbib-native author-date style and the closest available Chicago-style
substitute. The style is set once in `kr-common.tex`; papers call only
`\bibliography{...}` and must not issue their own `\bibliographystyle`.

## How to compile

```sh
export TEXINPUTS="$HOME/workspace/research-site/templates:"
cd /path/to/paper
pdflatex paper.tex
bibtex paper
pdflatex paper.tex
pdflatex paper.tex
```

Two pdflatex runs after bibtex settle cross-references and the table of
contents. A clean build shows no lines starting with `!` in `paper.log`.

## House typography rules enforced here

- 1in margins (`geometry`), 11pt article class.
- Captions: bold label, period separator ("Figure 1. Short declarative title",
  "Table 1. ...").
- Hyperlinks use `hidelinks` (no colored boxes).
- Block paragraphs: vertical space between paragraphs, no first-line indent.
- Tables use `booktabs` (`\toprule`, `\midrule`, `\bottomrule`); no vertical rules.
- No em dashes anywhere in template or paper source.
