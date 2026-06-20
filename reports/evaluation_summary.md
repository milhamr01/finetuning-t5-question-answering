# Evaluation Summary — T5 SQuAD Question Answering

## 1. Experiment Identity

- **Repository:** `finetuning-t5-question-answering`
- **Assignment:** UAS Deep Learning — Fine-Tuning HuggingFace Models
- **Task:** Task 2 — Encoder-Decoder / Seq2Seq Question Answering
- **Architecture:** Encoder-Decoder Transformer
- **Model:** `t5-base`
- **Dataset:** SQuAD (`rajpurkar/squad`)
- **Problem Type:** Generative question answering

## 2. Dataset Summary

The SQuAD dataset was loaded successfully from HuggingFace. The dataset contains context paragraphs, questions, and answer spans. The relevant fields are:

```text
id, title, context, question, answers
```

For T5 fine-tuning, each example was converted into a text-to-text format:

```text
Input : question: <question> context: <context>
Target: <answer>
```

The first available answer text was used as the target sequence.

### Dataset Subset Used

| Split | Size |
|---|---:|
| Training | 1000 |
| Validation | 200 |
| Test | 200 |

The full SQuAD training dataset contains 87,599 rows and the validation set contains 10,570 rows. A smaller subset was used to make fine-tuning practical in Google Colab.

## 3. Training Configuration

| Parameter | Value |
|---|---:|
| Model | `t5-base` |
| Epochs | 1 |
| Learning Rate | 3e-4 |
| Train Batch Size | 4 |
| Evaluation Batch Size | 4 |
| Weight Decay | 0.01 |
| Max Input Length | 384 |
| Max Target Length | 48 |
| Hardware | T4 GPU |

## 4. Training Result

| Metric | Value |
|---|---:|
| Training Loss | 0.332738 |
| Validation Loss | 0.468789 |
| Global Training Loss | 0.426763 |
| Runtime | 110.1166 seconds |

The notebook completed one training epoch successfully. The training process finished in approximately 110 seconds on the Colab runtime.

## 5. Evaluation Metrics

The generated answers were evaluated using:

- **Exact Match (EM):** Measures whether the generated answer exactly matches the reference answer after normalization.
- **Token-level F1-score:** Measures token overlap between generated and reference answers. This metric gives partial credit for semantically close or partially overlapping answers.

## 6. Final Test Metrics

| Metric | Value |
|---|---:|
| Exact Match | 0.59 |
| F1-score | 0.793421 |

## 7. Qualitative Prediction Examples

| Question | Prediction | Reference | Analysis |
|---|---|---|---|
| When did rapid warming begin and help vegetation? | 13,000 BP | 13,000 BP | Exact match |
| How long did it take Johnson to respond to Kennedy? | one week | approximately one week | Partially correct; high token overlap |
| What measurement of time is used in polynomial time reduction? | polynomial | polynomial time | Partially correct; missing one token |
| Who designed the Eiffel Tower? | Gustave Eiffel | Custom example | Correct custom inference |

## 8. Analysis

The Exact Match score of **0.59** indicates that 59% of predictions matched the reference answers exactly on the test subset. The F1-score of **0.793421** is higher because many generated answers were partially correct even when they did not exactly match the reference wording.

This behavior is common in generative question answering. For example, the prediction `one week` does not exactly match `approximately one week`, but the answer is semantically close and has strong token overlap. Similarly, `polynomial` partially matches `polynomial time`.

## 9. Conclusion

The fine-tuned T5-base model achieved **0.59 Exact Match** and **0.793421 F1-score** on the SQuAD test subset. The result is reasonable for a lightweight experiment using only 1,000 training samples and one training epoch.

T5-base is suitable for this task because its encoder-decoder architecture can read a question-context pair and generate an answer directly. For further improvement, the model can be trained on a larger subset, trained for more epochs, or tuned with different learning rates and generation settings.
