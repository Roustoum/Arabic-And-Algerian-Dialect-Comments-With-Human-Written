# Algerian Arabic & Darija Comment Summarization Dataset

An open, hand-authored dataset of informal Algerian Arabic / Darija social-media-style comments, each paired with a concise Arabic summary of its meaning, intention, sentiment, or communicative purpose.

## Overview

This dataset contains **67,519** text examples designed for Arabic text understanding and text-to-summary generation.

Each row pairs an informal comment (`input_text`) with a short Arabic description (`target_text`) that summarizes its main meaning, intention, sentiment, or communicative purpose. Related examples that express the same underlying meaning through different linguistic forms are grouped together (`group_id`, `variant_type`), so the dataset can be used to train and evaluate models that need to generalize across spelling, dialect, and script variation rather than memorizing surface wording.

The input texts cover the linguistic variation typical of Algerian online communication: Arabic script, Algerian Darija, Arabizi (Latin-script Darija), code-switching with French/English, informal spelling, repeated characters, emojis, and social-media vocabulary.

## Files

- `data.csv` — the complete labeled dataset: 67,519 rows, 5 columns.

## Columns

| Column | Type | Description |
|---|---|---|
| `id` | string | Unique identifier for the row. |
| `group_id` | string | Identifies rows that express the same underlying meaning (different linguistic variants of the same comment). Use this to build leakage-free train/test splits. |
| `variant_type` | string | Linguistic form of `input_text`: `original`, `arabizi`, `code_switch`, `spelling_noise`, or `social_noise`. |
| `input_text` | string | The Arabic / Algerian Darija / Arabizi / mixed-language comment. |
| `target_text` | string | A concise Arabic summary of the comment's meaning, intention, sentiment, or purpose. |

## Example

```csv
id,group_id,variant_type,input_text,target_text
ROW-3E988C37F2F2B7CA14,SRC-000815E5BAEA0A,original,ربي يوفقك خويا واصل,يدعو لصاحب المحتوى بالتوفيق ويشجعه على الاستمرار.
ROW-4BC0E7A33A70384947,SRC-000815E5BAEA0A,arabizi,rabi ywf9k khoya wasel,يدعو لصاحب المحتوى بالتوفيق ويشجعه على الاستمرار.
```

`target_text` is free-form text, not a class label: different comments that express the same intention may receive the same or a closely related summary, and evaluation should rely on semantic similarity rather than exact-string matching.

## Creation and Maintenance

This dataset is hand-authored: every `input_text` comment and every `target_text` summary was written from scratch by the maintainer, imitating realistic Algerian online writing rather than being scraped or sampled from real users or any existing corpus.

The dataset is actively maintained in this repository rather than being a single static drop:

- New comments and linguistic variants have been added by hand in subsequent revisions to broaden topic and dialect coverage.
- Existing rows have been reviewed and revised for semantic consistency between `input_text` and `target_text`.
- Group-level consistency is checked so that different variants of the same underlying comment keep compatible summaries.

Changes are tracked through this repository's commit history.

## License

Released as open source and free to use for research, education, model training, and competition purposes. Because the dataset is fully original and hand-written, it contains no third-party copyrighted text and no personal data belonging to real individuals.

*(Pick and state the exact license here, e.g. CC-BY-4.0, CC0, or MIT, and add a `LICENSE` file to the repo.)*

## Known Limitations

- **Synthetic, single-maintainer origin.** The data reflects the topics, phrasing, and intentions its author chose to write, not the natural frequency of those intentions in real Algerian online communication. It does not sample real user behavior.
- **Dialect coverage gaps.** Algerian Arabic has substantial regional and sociolinguistic variation; the varieties the author is most familiar with are over-represented, and Amazigh/Tamazight-influenced forms have limited coverage.
- **No standard Arabizi spelling.** Arabizi has no official transliteration standard, so the conventions used here reflect the author's own transliteration habits.
- **Annotation subjectivity.** `target_text` summaries are free-form, author-written descriptions, not objective labels — multiple valid summaries may exist for the same input, and there is no inter-annotator agreement to smooth out individual interpretation.
- **Variant duplication.** `variant_type` combined with `group_id` means several rows share the same or a closely related target. Use group-aware (not row-random) train/test splits to avoid leakage.
- **No demographic or temporal metadata.** There is no age, gender, geographic, or timestamp information, so representativeness across regions, demographics, or time cannot be verified.

## Intended Use

Suitable for: Algerian Darija understanding, Arabic text summarization, semantic description generation, intent interpretation, Arabizi and code-switching understanding, informal-language processing, and encoder-decoder / instruction-tuned / LLM fine-tuning.

Not intended as a representative sample of real-world Algerian online text: models trained on this data should not be assumed to transfer to formal Arabic, other Arabic dialects, or naturally occurring social-media text without additional real-world data.

When building training/validation/test splits, keep all rows sharing the same `group_id` in the same partition to avoid semantic overlap between splits.
