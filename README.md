# QA_SYSTEM_RVT — Question Answering over IBC Building Code

This project implements a local question answering system over the IBC 2018 (International Building Code) using ChromaDB, sentence embeddings, reranking, and GPT-4. It allows you to ask natural language questions about building regulations and get context-based, section-cited answers.

## Features

- Semantic search over IBC building code paragraphs using ChromaDB and SentenceTransformer
- Reranking of results using a cross-encoder (ms-marco-MiniLM-L6-v2)
- Answer generation via OpenAI GPT-4
- Automatic parsing and indexing of raw IBC text into structured paragraphs
- Persistent vector storage using ChromaDB
- Fully local processing (OpenAI is optional for generation)

## Project Structure

```
QA_SYSTEM_RVT/
├── .venv/                        # Virtual environment (gitignored)
├── __pycache__/
├── data/                         # Raw IBC text, parsed and cleaned data
│   └── IBC-2018 ICC International Building Code conv_02.txt
├── data_base/                    # Persistent ChromaDB storage
├── config.py                     # Contains API keys (ignored by git)
├── requirements.txt              # Python dependencies
├── how_to_install_venv.txt       # Setup instructions
├── qa_work_in_progress_*.ipynb   # Notebooks for dev work
├── question_answering_notebook.ipynb  # Main QA pipeline
└── README.md                     # Project documentation
```

## Setup

1. Clone the repository:

```bash
git clone https://github.com/yourname/QA_SYSTEM_RVT.git
cd QA_SYSTEM_RVT
```

2. Create a virtual environment:

```bash
python -m venv .venv
source .venv/bin/activate  # or .venv\Scripts\activate on Windows
```

3. Install dependencies:

```bash
pip install -r requirements.txt
```

4. Create `config.py` with your API keys:

```python
# config.py
TOKEN_HF = "your_huggingface_token"
OPENAI_API_KEY = "your_openai_api_key"
```

## How It Works

1. Parses raw IBC text from `.txt` and normalizes sections.
2. Splits text into paragraphs and assigns structured indices.
3. Embeds each paragraph using SentenceTransformer.
4. Stores embeddings in ChromaDB with metadata.
5. At query time:
   - Retrieves top-k relevant paragraphs by semantic similarity.
   - Optionally reranks results using a cross-encoder.
   - Sends the context to GPT-4 to generate the final answer with references.

## Example Queries

```python
search_ibc("minimum width of apartment door")
search_ibc("width of stairway in residential buildings")
```

```python
generate_ibc_answer_gpt4("what is the minimum width for corridors?")
```

## Dependencies

- chromadb
- torch, transformers
- sentence-transformers
- openai
- pandas, numpy
- IPython, re, json

## License

MIT License. See LICENSE file.
