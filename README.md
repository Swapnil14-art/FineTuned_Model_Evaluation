# BOQ Embedding Retrieval Benchmark

A benchmark for evaluating the effectiveness of fine-tuning **Qwen3-Embedding-0.6B** on Bill of Quantities (BOQ) document retrieval.

## Overview

This project compares a domain-specific fine-tuned **Qwen3-Embedding-0.6B** model against the original base model on a dense retrieval task for construction Bill of Quantities (BOQ) documents.

The objective is to determine whether domain-specific fine-tuning improves retrieval quality while maintaining retrieval efficiency.

---

## Dataset

- **Documents:** 142 BOQ PDF files
- **Indexed Items:** 26,228
- **Embedding Dimension:** 1024
- **Vector Database:** FAISS
- **PDF Extraction:** pdfplumber
- **Evaluation Queries:** 3,990
- **Ground Truth:** One relevant document per query

Each indexed item retains metadata linking it back to its original BOQ document.

---

## Evaluation Pipeline

```
BOQ PDFs
      │
      ▼
pdfplumber Extraction
      │
      ▼
Chunk Generation
      │
      ▼
Embedding Generation
      │
      ▼
FAISS Index
      │
      ▼
Synthetic Query Generation
      │
      ▼
Retrieval Evaluation
```

Both models were evaluated under identical conditions:

- Same corpus
- Same FAISS index
- Same embedding dimension
- Same normalization strategy
- Same query set

Only the embedding model differs.

---

## Metrics

The benchmark evaluates:

### Retrieval Metrics

- Recall@1
- Recall@3
- Recall@5
- Recall@10
- Precision@1
- Precision@3
- Precision@5
- Precision@10
- Mean Reciprocal Rank (MRR)
- Mean Average Precision (MAP)

### Similarity Metrics

- Average Similarity
- Maximum Similarity
- Minimum Similarity

### Efficiency Metrics

- Latency (ms/query)
- Throughput (Queries per Second)

### Query Difficulty

Performance is evaluated separately for:

- Easy queries
- Medium queries
- Hard queries

---

## Results

### Retrieval Performance

| Metric      | Base      | Fine-Tuned |
|-------------|----------:|-----------:|
| Recall@1    | 0.9527    | 0.9081     |
| Recall@3    | 0.9905    | 0.9426     |
| Recall@5    | 0.9942    | 0.9459     |
| Recall@10   | 0.9966    | 0.9495     |
| MRR         | 0.9717    | 0.9258     |
| MAP         | 0.9717    | 0.9258     |

The fine-tuned model consistently underperformed the base model across all retrieval metrics.

Average performance degradation ranged from approximately **4.5%–6.8%**.

---

### Similarity Metrics

Although retrieval quality decreased, embedding similarity increased after fine-tuning.

| Metric | Base | Fine-Tuned |
|--------|------:|-----------:|
| Average Similarity | 0.8103 | 0.8894 |
| Maximum Similarity | 0.9387 | 0.9675 |
| Minimum Similarity | 0.7517 | 0.8471 |

Higher similarity does **not necessarily** translate into better retrieval performance.

---

### Efficiency

| Metric | Base | Fine-Tuned |
|--------|------:|-----------:|
| Latency (ms) | 0.1042 | 0.1104 |
| Throughput (QPS) | 9596 | 9059 |

Fine-tuning introduced a small computational overhead while maintaining comparable efficiency.

---

### Query Difficulty

| Difficulty | Observation |
|------------|-------------|
| Easy | Significant performance drop |
| Medium | Slight improvement |
| Hard | Nearly identical |

This behavior may indicate that the fine-tuned model overfits to more complex BOQ patterns.

---

## Key Findings

- Fine-tuning did **not** improve retrieval quality.
- The base model achieved higher Recall, Precision, MRR, and MAP.
- Embedding similarity increased after fine-tuning.
- Increased similarity did not improve ranking quality.
- Efficiency differences between models were relatively small.
- Performance degradation was consistent across nearly all retrieval metrics.

---

## Limitations

Current evaluation has several limitations:

- Only 142 BOQ documents
- Approximately 3% query coverage
- Synthetic evaluation queries
- Single relevant document per query

Larger datasets and broader query coverage are recommended before drawing definitive conclusions.

---

## Future Work

- Increase corpus size
- Expand query coverage
- Use human-written evaluation queries
- Evaluate additional embedding models
- Explore hard negative mining
- Improve fine-tuning data quality
- Benchmark against other dense retrieval approaches

---

## Tech Stack

- Python
- Qwen3-Embedding-0.6B
- FAISS
- pdfplumber
- NumPy
- Pandas
- Matplotlib

---

## Repository Structure

```
.
├── data/
├── embeddings/
├── evaluation/
├── notebooks/
├── scripts/
├── results/
├── figures/
├── README.md
└── requirements.txt
```

---

## Conclusion

On this benchmark, the **base Qwen3-Embedding-0.6B** model outperformed the domain-specific fine-tuned model across every major retrieval metric. While fine-tuning increased embedding similarity, it did not improve retrieval effectiveness, highlighting the importance of evaluation design, training data quality, and corpus coverage when adapting embedding models for specialized retrieval tasks.