# Medly marking benchmark (public subset)

A self-contained subset of a double-marked GCSE mock-exam dataset for evaluating
automated (LLM) marking. Each student answer was marked independently by two examiners.

This subset has 480 answers across 20 questions: 240 typed English Language
essays and 240 handwritten Maths responses (rendered as images). Answers are not uniformly sampled from the population, but instead sampled to cover the full range of marks.

This dataset is a public companion subset to the report **LLM Performance on a Real,
Double-Marked GCSE Benchmark** (Fox et al., 2026). The full study covers 32,534
responses across 328 questions in five subjects; this release opens a reproducible
English + Maths slice. The paper PDF is included in this repository:
[`Fox-etal-2026-LLM-Exam-Marking-Benchmark.pdf`](Fox-etal-2026-LLM-Exam-Marking-Benchmark.pdf).

## Layout

```
medly-marking-benchmark/
├── dataset.csv                    # manifest: one row per answer
├── questions/
│   ├── english/<question_id>.json
│   └── maths/<question_id>.json
└── answers/
    ├── english/<answer_id>.txt    # typed essay text
    └── maths/<answer_id>.png      # rendered handwriting
```

## `dataset.csv`

| Column | Meaning |
|---|---|
| `answer_id` | Anonymous answer id (no student identity is retained) |
| `question_id` | Joins to `questions/<subject>/<question_id>.json` |
| `subject` | `english` or `maths` |
| `modality` | `typed` (English) or `handwritten` (Maths) |
| `answer_file` | Relative path to the answer text/image |
| `markmax` | Maximum mark for the question |
| `examiner_1_mark` / `examiner_2_mark` | The two independent examiner marks (ground truth) |
| `examiner_1_ao_marks` / `examiner_2_ao_marks` | Per-assessment-objective split (English essays only; blank otherwise), e.g. `AO5:16;AO6:7` |

## `questions/<subject>/<question_id>.json`

`question_id, markmax, question_stem, question_text, markscheme`.
The `question_stem` is the shared preamble
(e.g. the source text for English); `question_text` is the specific task. Text may contain
Markdown and LaTeX (`$...$`). Questions that refer to a figure also carry `question_image`
— a relative path to the diagram (PNG/SVG) in `questions/images/`.

## Notes

- **Anonymity:** No student identifiers are included; `answer_id`s are arbitrary. All
  names and addresses are redacted (replaced with placeholder tokens such as `[NAME]`).
- **Handwriting:** Maths PNGs are the exact rendered canvas a marker sees — the student's
  strokes on white, cropped to the bounding box of those strokes.
- **Floating handwriting artifacts:** a Maths image may contain isolated marks — a stray line, circle, underline or tick with nothing beside it. These appear because the student annotated the question text, but the question text is not captured in these images.
- **Low quality answers:** Low quality student answers are included in this subset. These include joke or incomplete answers.
- **Sensitive topics:** Some answers discuss sensitive topics. These may trigger content filters, which could limit some automated marking systems.

## Citation

If you use this dataset, please cite:

```bibtex
@techreport{fox2026marking,
  title       = {LLM Performance on a Real, Double-Marked GCSE Benchmark},
  author      = {Fox, Malachy and Samra, Kavi and Jung, Paul},
  institution = {Medly AI},
  year        = {2026},
  url         = {https://github.com/medlyai/medly-marking-benchmark}
}
```

## License

This dataset is released under **[Creative Commons Attribution 4.0 International
(CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/)**. You may share and
adapt the material for any purpose, including commercially, provided you give
appropriate credit (cite Fox et al., 2026).
