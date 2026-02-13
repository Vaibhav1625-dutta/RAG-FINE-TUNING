# Advance RAG Studio

Advance RAG Studio is a production-style, multimodal Retrieval-Augmented Generation (RAG) application built with Streamlit. It supports document ingestion, vector search, reranking, and grounded chat over text, audio transcripts, and image-derived context.

## Highlights

- Multimodal ingestion: `.txt`, `.md`, `.pdf`, `.wav`, `.mp3`, `.mp4`, `.m4a`, `.flac`, `.ogg`, `.png`, `.jpg`, `.jpeg`, `.webp`
- Audio transcription with Whisper (when `ffmpeg` is available)
- Image captioning and image-aware retrieval using a Groq-compatible vision model
- Embeddings powered by Jina Embeddings v4
- FAISS vector index for low-latency semantic retrieval
- Optional cross-encoder reranking for higher retrieval precision
- Grounded-answer policy (answers are restricted to retrieved context)
- Session memory, source visibility, and latency breakdown in the UI

## Architecture

- `main.py`: core RAG engine (ingestion, chunking, embedding, indexing, retrieval, reranking, generation)
- `app.py`: Streamlit interface (workflow controls, chat, diagnostics, visual styling)
- `requirements.txt`: Python dependencies
- `runtime.txt`: Python runtime pin for cloud compatibility

## Quick Start

### 1. Create and activate a virtual environment

Windows (PowerShell):

```bash
python -m venv .venv
.venv\Scripts\Activate.ps1
```

macOS/Linux:

```bash
python -m venv .venv
source .venv/bin/activate
```

### 2. Install dependencies

```bash
pip install -r requirements.txt
```

### 3. Configure API keys

Windows (PowerShell):

```bash
$env:GROQ_API_KEY="your_groq_api_key_here"
$env:JINA_API_KEY="your_jina_api_key_here"
```

macOS/Linux:

```bash
export GROQ_API_KEY="your_groq_api_key_here"
export JINA_API_KEY="your_jina_api_key_here"
```

You can also provide both keys in the Streamlit sidebar at runtime.

### 4. Run the application

```bash
streamlit run app.py
```

## Example Backend Usage

```python
from main import AdvanceRAG

rag = AdvanceRAG(groq_api_key="your_groq_api_key", jina_api_key="your_jina_api_key")
rag.ingest_txt_bytes(open("notes.txt", "rb").read(), source="notes.txt")
rag.build_index()

response = rag.answer("Summarize the key topics.")
print(response["answer"])
```

## Operational Notes

- Do not hardcode API keys in source files.
- First-time Whisper usage may trigger model downloads and increase startup time.
- If `ffmpeg` is unavailable, audio files are skipped and the app remains functional.
- Reranking improves answer quality but adds latency on first model load.
- For larger corpora, tune `chunk_size`, `top_k`, and `candidate_k` for speed/quality tradeoffs.

## Deploy on Streamlit Community Cloud

1. Push this repository to GitHub.
2. Create a new Streamlit app and set `app.py` as the entrypoint.
3. Add secrets in the app settings:

```toml
GROQ_API_KEY = "your_groq_api_key_here"
JINA_API_KEY = "your_jina_api_key_here"
```

4. Deploy.

`runtime.txt` pins Python to improve package wheel compatibility in cloud environments.

