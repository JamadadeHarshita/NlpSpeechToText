# Lecture Summarization | NLP Project

## Objective
To build a modular and scalable pipeline that semantically segments and summarizes long-form lecture or talk audio. The goal is to overcome the limitations of traditional structure-agnostic summarization methods by introducing topic-aware processing using embeddings and clustering.

---

## Pipeline Overview

### 1. Audio Acquisition
- Tool Used: `yt_dlp` to download audio from YouTube.
- Audio Format: `.wav`
- Post-processing: `ffmpeg` for audio extraction; `soundfile` for managing audio writing.

### 2. Transcription
- Model Used: OpenAI's `whisper` (base model).
- Chunking Strategy: Audio split into 60-second segments.
- Benefit: Enables parallel, memory-efficient transcription.

### 3. Chunking & Preprocessing
- Tokenizer: `nltk` sentence tokenizer.
- Chunk Strategy: 5-sentence chunks with 1-sentence overlap to preserve context flow.

### 4. Embedding & Similarity
- Model: `sentence-transformers/all-MiniLM-L6-v2`
- Similarity Metric: Cosine similarity on normalized vectors.
- Output: Pairwise similarity matrix, visualized as a heatmap.

### 5. Louvain Clustering for Topic Segmentation
- Graph Construction: Nodes = chunks; edges = high similarity (threshold > 0.6).
- Algorithm: Louvain Community Detection.
- Outcome: Discovery of coherent topic clusters.

### 6. Topic Labeling
- Tool: `KeyBERT`
- Output: Top 3 keywords per topic.

  Example Topics:
  - Topic 0: ai, phd, masters  
  - Topic 1: retrieval, topics, multimodality  
  - Topic 2: language, ai, models  

### 7. Final Tags for Video
- Extracted unique, high-frequency keywords across topics to assign global tags:


### 8. Semantic Summarization
- Model Used: `HuggingFaceH4/zephyr-7b-alpha`
- Summary Style: Teaching-assistant style explanation for each topic cluster.
- Example Summary:
  "This lecture section focused on optimizing end-to-end retrieval-augmented systems..."

---

## Challenges & Improvements

### Problem: Topic Misalignment with LDA
- LDA misclassified jokes or asides as topics.

### Solution: Louvain Clustering
- Preserved semantic flow.
- Handled scattered yet related topic mentions better.

---

## Limitations
- No formal evaluation using ROUGE or human judgments.
- No async batching in current implementation (affects scalability).
- Occasional hallucinations from Zephyr model on rare prompts.

---

## Future Work
- Add evaluation via GPT-based or human annotation.
- Integrate `asyncio` for parallel chunk processing.
- Extend support to other domains like podcast or meeting summarization.

---

