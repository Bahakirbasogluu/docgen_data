# Multilingual Technical Support Dataset (TR / EN)

A multilingual, multi-turn **technical support dialogue dataset**. Each sample contains a complete issue-resolution thread between an end user/customer and a Technical Support Agent (TSA), together with the issue title, the related product/application category, a sentiment label, and a reference summary. The dataset is designed to evaluate **issue classification, title generation, sentiment analysis, and summarization** in enterprise technical-support scenarios.


## Contents

| File | Language | Samples | Brands / Categories |
|------|----------|---------|---------------------|
| `tr_data.json` | Turkish | 106 | 13 |
| `en_data.json` | English | 235 | 26 |
| **Total** | — | **341** | — |

## Schema (per record)

| Field | Type | Description |
|-------|------|-------------|
| `source` | string | General source of the sample (e.g., Stack Overflow, Reddit, Microsoft, tech forum). Individual URLs were removed for privacy and normalization. |
| `product` | string | Related product/application category → issue classification label |
| `title` | string | Title summarizing the issue → title generation target |
| `question` | string | The user's initial problem description |
| `answers` | list | Dialogue turns; each is `{ "answer_text": "..." }`. Represents the multi-turn conversation flow. |
| `solution` | string | The accepted/final resolution |
| `sentiment` | string | Customer tone in the question: `negative`, `neutral`, `positive` |
| `reference_summary` | string | Concise reference summary of the problem and resolution |
| `language` | string | `tr` or `en` |

## Task Coverage

- Issue Classification
- Title Generation
- Sentiment Analysis
- Reference Summarization

## Notes

- Privacy-preserving and anonymized.
- Internal corporate correspondence is not included.
- Designed for evaluation and few-shot prompting.
