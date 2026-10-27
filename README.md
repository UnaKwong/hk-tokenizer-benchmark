# Hong Kong Tokenizer Benchmark

How many tokens do popular tokenizers need for the **same meaning** in English, Traditional Chinese, Simplified Chinese and **Cantonese**, and is the original text preserved when the tokens are decoded again?

This project compares six tokenizers on the FLORES-200 parallel benchmark and on a small hand-written Hong Kong test set (Cantonese, Cantonese–English code-mixing and Python code).

## Tokenizers

| Company | Tokenizer | Algorithm | Vocabulary |
|---|---|---|---|
| OpenAI | r50k_base (GPT-3) | Byte-level BPE | 50,257 |
| OpenAI | cl100k_base (GPT-4) | Byte-level BPE | 100,277 |
| OpenAI | o200k_base (GPT-4o) | Byte-level BPE | 200,019 |
| Alibaba | Qwen2.5 | Byte-level BPE | 151,665 |
| Google | mBERT (bert-base-multilingual-cased) | WordPiece | 119,547 |
| Meta | XLM-R (xlm-roberta-base) | SentencePiece Unigram | 250,002 |

Only tokenizer files are downloaded. No model weights are used, so this measures tokenization, not answer quality.

## Key results (FLORES-200 devtest, 1,012 sentences)

- **Cantonese premium vs English** fell from **3.18×** (r50k) to **2.28×** (cl100k) and **1.54×** (o200k).
- **Cantonese still needs more tokens than Traditional Chinese** in all four byte-level tokenizers: +2.3% (r50k), +6.3% (cl100k), +8.5% (o200k), +11.6% (Qwen2.5). All 95% bootstrap confidence intervals are above 1.0.
- **Fewer tokens can hide information loss**: mBERT mapped 1,279 Cantonese characters to `[UNK]`, and only 32% of Cantonese sentences were rebuilt correctly (97% for Traditional Chinese). mBERT and XLM-R also dropped line breaks and indentation in Python code.

## Files

| Path | Content |
|---|---|
| `tokenizer_benchmark.ipynb` | Full analysis with outputs (Sections 1–12) |
| `sentences.json` | Hand-written Hong Kong test set: 5 parallel sets + 3 Python snippets |
| `requirements.txt` | Python packages |
| `output/flores_summary.csv`, `output/flores_bootstrap.csv` | FLORES totals, premiums and 95% bootstrap CIs (aggregates only) |
| `output/results.csv`, `output/summary.csv` | Per-sample and summary results for the Hong Kong test set |
| `output/mitigation.csv`, `output/vocab_extension.csv` | Space-removal and vocabulary-extension experiments |
| `output/vocab_sizes.csv`, `output/environment.txt` | Tokenizer details, package versions and pinned revisions |
| `output/chart_*.png` | Figures |
| `output/token_boundaries*.html` | Token-boundary views (download and open in a browser) |

## How to run

1. Python 3.10 or later: `pip install -r requirements.txt`
2. FLORES+ is a gated dataset. Accept its terms at https://huggingface.co/datasets/openlanguagedata/flores_plus and log in with `huggingface-cli login`. No access token is stored in this repository.
3. Open `tokenizer_benchmark.ipynb` and run all cells. Results are written to `output/`.

Dataset and tokenizer revisions are pinned in `output/environment.txt` so the numbers can be reproduced.

## Data note

FLORES sentences are not redistributed here. The output files contain only aggregate statistics for FLORES; example sentences come from the hand-written test set.
