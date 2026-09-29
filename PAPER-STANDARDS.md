# Research paper standards

Two tracks. Every new paper must comply with its track from the first draft.
Existing papers that do not comply get rebuilt. No exceptions.

## Track A: Scientific, mathematical, physics papers

Model: standard journal article (LaTeX).

Required title page:
- Title
- "Kilambi Research" byline (see Both tracks)
- Program desk line: "Kilambi Research -- <Program> Program"
- Paper number: "Kilambi Research Working Paper No. KR-WP-2026-0X", matching the website's paper numbering
- Contact email
- Date
- Abstract (what was done, what was found, one paragraph)
- Keywords

Table of contents included in every paper.

Required structure:
1. Introduction (question, why it matters, what this paper contributes)
2. Theory / Methods (enough detail to reproduce)
3. Results (numbered figures and tables, every one referenced and interpreted in the prose)
4. Discussion (what the results mean, what would prove them wrong)
5. Conclusion (no uplift, no throat-clearing)
6. References
7. Appendices (derivations, robustness checks, data details)

Typography: LaTeX, 11pt article class or equivalent, numbered sections,
numbered figures and tables with captions, standard math typesetting.
Every quantitative claim traces to a source or a derivation (audit pass).

## Track B: Economics and market-research papers

Model: NBER working paper.

Required title page:
- Title
- "Kilambi Research" byline (see Both tracks)
- Program desk line: "Kilambi Research -- <Program> Program"
- Paper number: "Kilambi Research Working Paper No. KR-WP-2026-0X", matching the website's paper numbering
- Contact email
- Date
- Abstract
- Keywords
- JEL codes (required)

Table of contents included in every paper.

Required structure:
1. Introduction (the question, why it matters now, what this paper does, preview of results). Ends with a roadmap paragraph: "The rest of the paper proceeds as follows..." (required).
2. Institutional background (how the market actually works, for a non-specialist)
3. Related literature (required)
4. Data (sources, coverage, sample period, definitions; a summary-statistics table is required, not optional)
5. Methodology (exact computations, what is estimated and how)
6. Results (tables and figures, every one interpreted in prose; no chart without a read, no number without a source)
7. Limitations and robustness (what the data cannot show, where the results are fragile)
8. Conclusion
9. References
10. Data appendix (series definitions, source table)

Typography: LaTeX, 11pt, numbered sections, numbered tables and figures
with full captions including source lines. Tables for headline numbers;
charts for patterns over time or across groups. Never a charts-only paper,
never a text-only paper.

## Both tracks

- Run ~/workspace/ai-mannerisms-check.md on the full manuscript and fix every failure.
- No em dashes anywhere.
- Dry research-lab voice. Bottom line first.
- Disclose data vintage, stale data, weak sources, and definition mismatches.
- Byline: every paper carries "Kilambi Research" as the byline. Retire all first-name-only and mixed conventions. The program name goes on the desk line under the byline.
- Voice: consistent desk voice across the series. "We" may refer to the research desk's analytical work. Convert personal-performance claims ("I collected", "the author ran") to desk or impersonal phrasing. The genre default is "we", used consistently; there is no fully-impersonal mandate.
- Production note: every paper states honestly, in the acknowledgments or methods, how the analysis was produced. Required wording pattern: produced by Kilambi Research's quantitative research pipeline with AI assistance; all analysis verified against source data. Never claim a person performed steps they did not (e.g. the fuel paper's "All simulations were performed by the author" must describe the pipeline instead).
- Citations: Chicago author-date via natbib; alphabetical reference list with hanging indent; DOI link where available; zero orphan keys (every bib key must have at least one \cite).
- Evidence grading: every reported number is tagged by provenance and by source quality, using this vocabulary and no other. Provenance: Observed (taken directly from a published source), Reconstructed (computed by the desk from source data), Modelled (output of the desk's model or simulation), Assumed (explicit assumption). Source quality: A (official/primary source), B (reputable secondary), C (provisional/estimated). Example: "US$1.0B (Assumed, C)".
- Falsifiability: every paper states what evidence would prove its main conclusions wrong, in a short falsifiability paragraph or subsection. Where true, state plainly: "This is a retrospective analysis; there was no pre-registered plan."
- Hedge with the number: uncertainty and key caveats appear in the SAME sentence as the headline number, not paragraphs later.
- The site record page stays arXiv-minimal (title, byline, date, abstract, PDF link).
  The PDF is the paper and must stand alone.

## Table standard (both tracks)

- Booktabs formatting: \toprule, \midrule, \bottomrule. No vertical rules.
- 2-3 significant digits. Round, don't truncate; recompute from source data, never hand-edit a printed value.
- Every table gets Notes: and Source: lines beneath it.
- Standard errors in parentheses.
- No significance stars or asterisks.

## Figure standard (both tracks)

Every figure must be produced with the shared institutional style at
`~/workspace/research-site/chart_style.py`:

    import chart_style; chart_style.apply()

- Okabe-Ito palette (colorblind-safe, print-friendly), one color per series,
  pinned consistently across every figure in the paper.
- Sans-serif type (Arial/Helvetica/DejaVu Sans), 8.5-11pt at final size:
  title 11pt bold, labels 9.5pt, ticks and legend 8.5pt. Units in axis
  labels, e.g. "GDP per capita (USD)".
- Horizontal gridlines only (#D9D9D9), drawn behind data. No vertical
  gridlines on time series.
- Top and right spines off; left/bottom #333333, 0.8pt. Outward ticks.
- Data lines 1.8pt; markers 4-5pt on sparse series.
- Never encode by color alone: add line styles, markers, or direct
  end-of-line labels so the figure survives grayscale printing.
- Prefer direct series labels over legends for 3 or fewer series; frameless
  legend otherwise, never covering data.
- Output 300 DPI PNG (or vector PDF), saved with a tight bounding box
  (bbox_inches="tight") and white figure facecolor; chart_style.save() does
  both. Do not override savefig.bbox or savefig.facecolor.
- Captions live in LaTeX, not in the figure: "Figure N. Short declarative title" with Source and Note lines.
- No default matplotlib styling, ever. Regenerating a figure must reproduce
  the underlying data exactly; only the styling changes.
- Call chart_style.qa_check(fig) on every figure before saving. It raises on
  H1/H2 failures and prints advisory warnings for H3/H6/H7/H11; warnings are
  reviewed by eye at final printed size, never silently suppressed.

## Chart honesty rules (both tracks)

Added 2026-09-29 after a clipped-line chart shipped: if you cannot see a
line or a point on the chart, that is a bug, not a style choice.

- **H1 -- No clipped data.** Every plotted point lies inside the axes view
  limits. Set limits from the data, never from a round number. Call
  `chart_style.qa_check(fig)` on every figure before saving; it raises on
  violations.
- **H2 -- Bars start at zero.** Bar length encodes magnitude; a truncated
  baseline misreports the data. Line charts may use a non-zero baseline
  only with the real range shown on the axis.
- **H3 -- No dual y-axes** that invite reading a correlation the numbers do
  not support. Use stacked panels sharing an x-axis. (qa_check warns on any
  twin axis; the warning is advisory -- H3 bans only dual axes that invite
  a correlation reading.)
- **H4 -- No silent interpolation.** Break lines (NaN gap) where
  observations are missing: surplus years, data gaps, series breaks. Time
  runs left to right on an even scale; gaps are shown as gaps, never
  connected across.
- **H5 -- Small multiples share axis ranges** when the layout invites
  comparison.
- **H6 -- Every axis labeled with units;** every figure's data source
  stated in the caption Note line. (qa_check warns when a label is missing
  entirely; checking that a label carries units is human review.)
- **H7 -- At most ~6 series per panel;** more goes to small multiples or
  gets direct labels. Legends never obscure data.
- **H8 -- Never encode by color alone** (see Figure standard above).
- **H9 -- Read the rendered PNG at final printed size** before shipping.
  Code inspection alone does not pass QA.
- **H10 -- One dwarf series must not flatten the rest silently:** rescale
  thoughtfully (indexed, split panel, annotated cap), never clip.
- **H11 -- No text may cover data:** legends, annotations, textboxes, and
  labels never sit on top of data lines, points, or bars. Legends go
  outside the axes (bbox_to_anchor) or in a corner verified empty of
  data; series may be labeled directly at line ends instead. Annotations
  sit in whitespace the data leaves, with a short leader line when the
  target is distant. If no honest empty placement exists, the note moves
  to the figure caption or a footnote below the axes -- never an opaque
  box stamped over the data. (qa_check warns on every inside-axes legend;
  the warning is advisory -- a corner verified empty of data by eye remains
  permitted.)

Enforcement: qa_check raises AssertionError on H1/H2 failures and prints
advisory warnings for H3, H6, H7, and H11. H4, H5, H8, H9, H10, the
no-em-dash rule, the mannerisms checklist, the data-identity rule
(regenerating a figure must reproduce the underlying data exactly), and the
charts-only/text-only ban are enforced by human review of the rendered
output, not by qa_check.
