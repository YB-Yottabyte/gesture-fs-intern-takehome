# Full Stack Intern Take-Home Assignment

## 🎯 Objective

Build an **interactive Q&A chatbot** for a marketing agency using LangChain and a local LLM. A potential client can ask questions about services, pricing, and process — and get answers pulled from the agency's docs.

No API keys. No GPU. Everything runs locally.

---

## ⏱️ Time Expectation

~1-2 days

## 📋 What You'll Build

An interactive CLI where a client asks questions and gets answers:

```
> How much does the Growth package cost?

📄 Sources:
  1. GROWTH PACKAGE — $5,500/month. Best for scaling businesses...

💬 Answer: The Growth package costs $5,500 per month.

> Can I cancel early?

📄 Sources:
  1. Early termination before the minimum commitment requires...

💬 Answer: Yes, with 50% of the remaining contract value.
```

---

## 🧰 Stack

| Component    | Library / Tool                          |
| ------------ | --------------------------------------- |
| Framework    | LangChain (v0.3.x)                      |
| Embeddings   | HuggingFace (`all-MiniLM-L6-v2`, local) |
| Vector Store | FAISS (local)                           |
| LLM          | `google/flan-t5-base` (local, CPU)      |
| Testing      | pytest                                  |

---

## 🚀 Getting Started

1. **Fork** this repo to your own GitHub account
2. Clone your fork and `cd` into it
3. Set up the environment:

```bash
python3 -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate
pip install -r requirements.txt
```

4. Run the tests to verify the implementation:

```bash
pytest tests/ -v
```

> First run downloads two models (~1.2GB total). Cached after that.

---

## 📝 Implementation

The response layer is implemented in `src/pipeline.py` with 2 main parts.

The document loading, chunking, embeddings, and vector store are **already built** in `knowledge_base.py`. That file remains unchanged.

### 1. Question Retrieval and Answer Generation

The `ask_question()` function:

1. Searches the vector store for the 3 most relevant chunks
2. Combines their text into a context string
3. Plugs it into the provided prompt template
4. Calls the LLM and returns the answer + sources

**Hint — searching the vector store:**

```python
docs = vector_store.similarity_search("some query", k=3)
text = docs[0].page_content  # the actual text
```

**Hint — calling the LLM:**

```python
result = llm("some prompt")
answer = result[0]["generated_text"]
```

### 2. Interactive CLI

The `main()` function:

1. Builds the knowledge base and loads the LLM (helpers provided)
2. Takes user input
3. Calls ask_question() and prints the response
4. Exits on `quit`

---

## ✅ Evaluation

```bash
pytest tests/ -v
```

| Criteria                       | Weight |
| ------------------------------ | ------ |
| All tests pass                 | 40%    |
| Code clarity and structure     | 25%    |
| Correct retrieval + generation | 25%    |
| Bonus (see below)              | 10%    |

### Bonus (optional)

- Error handling (empty input, missing files)
- `--query` CLI argument for single-question mode
- Additional test cases
- Type hints

---

## 📁 Project Structure

```
gesture-fs-intern-takehome/
├── README.md
├── requirements.txt
├── data/
│   ├── services.txt              ← agency service descriptions
│   ├── pricing.txt               ← packages and pricing
│   ├── faq.txt                   ← client FAQ and process
│   ├── company_handbook.txt      ← company policies and information
│   └── product_faq.txt           ← product-related FAQ
├── src/
│   ├── __init__.py
│   ├── knowledge_base.py         ← PRE-BUILT (do not modify)
│   └── pipeline.py               ← Q&A response and CLI implementation
└── tests/
    ├── __init__.py
    └── test_pipeline.py
```

---

## ⚠️ Troubleshooting

**`command not found: python`** — Use `python3`.

**`ModuleNotFoundError`** — Activate venv and run `pip install -r requirements.txt`.

**Slow first run** — Models download once (~1.2GB), then cached.

---

## ❓ FAQ

**Do I need an API key?** No.

**What Python version?** 3.10+

**Can I modify `knowledge_base.py`?** No.

---

## Bonus Features Implemented

- Handles empty input and missing files
- Supports `--query` for a single question
- Includes extra pytest tests
- Adds type hints to key functions

### Interactive Mode

Start the interactive chatbot:

```bash
python -m src.pipeline
```
Type `quit` to exit.

### Single-Question Mode

Run one question directly from the command line:

```bash
python -m src.pipeline --query "How much does the Growth package cost?"
```

---

Good luck! 🚀
