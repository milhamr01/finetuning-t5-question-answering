# Fine-Tuning T5 for Question Answering

This repository was created for the **UAS Deep Learning — Fine-Tuning HuggingFace Models** assignment.

## Project Purpose

The purpose of this repository is to build an end-to-end encoder-decoder Transformer pipeline for generative question answering using HuggingFace. The project fine-tunes **T5-base** on the **SQuAD** dataset to generate answer text from a given question-context pair.

## Project Overview

This project covers the full sequence-to-sequence NLP pipeline:

1. SQuAD dataset loading and inspection
2. Dataset subset creation for practical Colab training
3. Conversion of SQuAD examples into T5 text-to-text format
4. Input and target tokenization
5. T5-base model loading
6. Sequence-to-sequence fine-tuning using HuggingFace `Seq2SeqTrainer`
7. Answer generation on test examples
8. Evaluation using Exact Match and token-level F1-score
9. Inference demo using a custom question-context pair

## Assignment Mapping

- **Task:** Task 2 — T5 Question Answering
- **Architecture Type:** Encoder-Decoder / Sequence-to-Sequence Transformer
- **Model Used:** T5-base (`t5-base`)
- **Dataset:** SQuAD (`rajpurkar/squad`)
- **Problem Type:** Generative Question Answering
- **Repository Name:** `finetuning-t5-question-answering`

## Repository Structure

```text
.
├── README.md
├── requirements.txt
├── notebooks/
│   └── 02_t5_squad_question_answering.ipynb
└── reports/
    └── evaluation_summary.md
```

## Notebook Description

### `notebooks/02_t5_squad_question_answering.ipynb`

This notebook contains:

- theoretical explanation of T5 for question answering,
- SQuAD dataset loading,
- dataset field inspection,
- subset creation for train, validation, and test,
- conversion of question-context-answer examples into text-to-text format,
- tokenization of input prompts and target answers,
- T5-base model loading,
- sequence-to-sequence training configuration,
- fine-tuning using `Seq2SeqTrainer`,
- generated answer evaluation,
- Exact Match and F1-score calculation,
- inference demo with a custom context and question.

## Dataset Configuration

The SQuAD dataset loaded successfully with these fields:

```text
id, title, context, question, answers
```

The experiment used a lightweight subset for practical training in Google Colab with T4 GPU.

| Split | Size |
|---|---:|
| Training | 1000 |
| Validation | 200 |
| Test | 200 |

## Training Configuration

| Parameter | Value |
|---|---:|
| Model | `t5-base` |
| Epochs | 1 |
| Learning Rate | 3e-4 |
| Train Batch Size | 4 |
| Eval Batch Size | 4 |
| Weight Decay | 0.01 |
| Max Input Length | 384 |
| Max Target Length | 48 |
| Hardware | T4 GPU |

## Final Results

### Training Result

| Metric | Value |
|---|---:|
| Training Loss | 0.332738 |
| Validation Loss | 0.468789 |
| Global Training Loss | 0.426763 |
| Runtime | 110.1166 seconds |

### Test Subset QA Metrics

| Metric | Value |
|---|---:|
| Exact Match | 0.59 |
| F1-score | 0.793421 |

## Result Analysis

The fine-tuned T5-base model achieved an **Exact Match score of 0.59** and an **F1-score of 0.793421** on the SQuAD test subset. Exact Match is strict because the generated answer must match the reference answer exactly. F1-score is more flexible because it gives partial credit when the generated answer overlaps with the reference answer.

Some generated answers were exactly correct, such as `13,000 BP` compared with the reference `13,000 BP`. Some answers were partially correct, such as `one week` compared with `approximately one week`, and `polynomial` compared with `polynomial time`. These examples explain why the F1-score is higher than the Exact Match score.

## Inference Demo Result

The trained model was also tested on a custom question-context pair:

| Context Topic | Question | Generated Answer |
|---|---|---|
| Eiffel Tower | Who designed the Eiffel Tower? | Gustave Eiffel |

The inference result shows that the fine-tuned T5 model can generate a correct answer for a simple unseen question-context pair.

## How to Run

### Google Colab Recommended

1. Open `notebooks/02_t5_squad_question_answering.ipynb` in Google Colab.
2. Enable GPU:

```text
Runtime > Change runtime type > T4 GPU
```

3. Run all cells from top to bottom.
4. Download the executed notebook.
5. Clean notebook widget metadata if GitHub displays an `Invalid Notebook` message.
6. Upload the fixed notebook back to this repository.

### Local Environment

```bash
python -m venv venv
```

Activate the environment:

```bash
# Windows
.\venv\Scripts\activate

# macOS/Linux
source venv/bin/activate
```

Install dependencies:

```bash
pip install -r requirements.txt
```

Run Jupyter Notebook:

```bash
jupyter notebook
```

## Identification

- **Name:** MUHAMMAD ILHAM RAYANDA
- **Class:** Deep Learning
- **NIM:** Fill your NIM here
- **Group:** Fill your group name/member list here

## Conclusion

This repository successfully implements an end-to-end T5-base fine-tuning pipeline for SQuAD question answering. The model achieved **0.59 Exact Match** and **0.793421 F1-score** on the test subset. The result shows that T5-base is effective for generative question answering because it can read a question-context pair and generate answer text directly.

## Notes

This repository is prepared as a reproducible academic deep learning project. The code is original/adapted for the assignment and is intended for learning purposes.
