# AI Document Chat (RAG Q&A)

TL;DR
-----
Upload a PDF, ask natural-language questions, and get concise answers with page-level citations. Built with Streamlit (UI), LangChain (orchestration), FAISS (vector store) and OpenAI/HuggingFace for embeddings & LLM.

Live demo: [add demo link here]  
Source code: [add repo link here]

Why this project
----------------
Long documents are hard to search quickly. This app ingests documents, creates semantic embeddings, and uses Retrieval-Augmented Generation (RAG) to answer questions with explicit source references (page numbers and excerpts). Great for research papers, manuals, policies, and reports.

Features
--------
- Upload PDF(s) and extract text with page numbers
- Chunking and embedding of document text
- Local FAISS vector store for retrieval
- Natural language Q&A powered by an LLM
- Answers include page references and source excerpts
- Simple Streamlit UI: document viewer + chat interface
- Session chat history and multi-document support (can be extended)

Built with
----------
- Streamlit — web UI
- LangChain — orchestration & chains
- FAISS — local vector database
- OpenAI / Hugging Face — embeddings & LLMs
- PyPDF2 (or pdfplumber) — PDF parsing
- Python 3.10+

Quick start (local)
-------------------
1. Clone this repo
```bash
git clone <your-repo-url>
cd ai-document-chat
```

2. Create and activate a virtual environment
```bash
python -m venv venv
source venv/bin/activate        # macOS / Linux
venv\Scripts\activate           # Windows
```

3. Install dependencies
```bash
pip install -r requirements.txt
```

4. Create a `.env` file in project root and add API keys
```bash
OPENAI_API_KEY=sk-...
# Optional (if using Hugging Face or other providers)
# HUGGINGFACEHUB_API_TOKEN=hf_...
```

5. Run the app
```bash
streamlit run app.py
```

6. Open the displayed URL (usually http://localhost:8501)

Environment variables
---------------------
- `OPENAI_API_KEY` — OpenAI API key for embeddings & LLMs (if using OpenAI)
- `HUGGINGFACEHUB_API_TOKEN` — (optional) Hugging Face token if using HF inference/embeddings
- Any other provider keys you want to use (Anthropic, Cohere, etc.) — update code accordingly

Project structure
-----------------
```
ai-document-chat/
├── app.py                 # Streamlit application
├── utils/
│   ├── pdf_processor.py   # PDF extraction & chunking
│   ├── vector_store.py    # FAISS operations
│   └── qa_chain.py        # LangChain Q&A logic
├── data/
│   ├── uploads/           # Uploaded PDFs
│   └── vector_db/         # Persisted FAISS index
├── requirements.txt
├── .env                   # API keys (not checked in)
├── .gitignore
└── README.md
```

How it works (high-level)
-------------------------
1. Upload a PDF → extract text per page.
2. Chunk text into overlapping segments (keeps context manageable).
3. Compute embeddings for each chunk and store them in FAISS.
4. On user question: retrieve the top-k relevant chunks from FAISS.
5. Pass retrieved context to the LLM with a prompt that asks it to answer and cite pages.
6. Display the answer and show the page numbers + excerpts used.

Best practices & tips
--------------------
- Chunk size: 800–1200 characters with 150–300 overlap works well for accuracy.
- `k` in retrieval: 3–6 is a good starting point — larger k can increase context but slow things down.
- Use temperature=0 (deterministic) for factual Q&A.
- Test with a few known questions to validate citation behavior.
- If the model hallucinates, show more context or use a stricter prompt that asks the model to reply "I don't know" when unsure.

Extending features (ideas)
--------------------------
- Highlight source excerpts in a PDF viewer (e.g., render pages as images + overlay).
- Multi-document conversational context (switch between docs or combine sources).
- Add caching & persistence for uploaded docs and FAISS indexes.
- Add user authentication and private storage for docs.
- Add question refinement / clarification follow-ups.
- Support other file types: DOCX, PPTX, plain text, HTML.
- Deploy to Streamlit Cloud, Hugging Face Spaces, or Docker + VPS for production.

Deployment
----------
- Streamlit Cloud: easiest for quick demos (connect repo, set env vars).
- Hugging Face Spaces: for Gradio/Streamlit demos — set secrets and push.
- Docker: create a Dockerfile to containerize app for more control.
- For production: consider using a hosted vector DB (Weaviate, Milvus, Pinecone) and a faster LLM endpoint; add authentication & logging.


Troubleshooting
---------------
- No text extracted from PDF: try `pdfplumber` instead of `PyPDF2` (some PDFs store text as images).
- Slow responses: reduce `k`, switch to smaller embeddings, or cache embeddings/indexes.
- Bad or hallucinated answers: increase retrieved context, lower temperature, and add a guardrail prompt telling the model to say “I don't know” when unsure.

Security & privacy
------------------
- Uploaded documents may contain sensitive data — do not use production keys or real secrets when experimenting publicly.
- For private deployments, enable authentication and secure storage (AWS S3 + IAM, encrypted DB, etc.).

License
-------
This project is provided under the MIT License. See LICENSE file.

Credits
-------
- Built with help from LangChain examples and many open-source projects in the RAG space.
- Author: Pridhvi2297

Get in touch
------------
- GitHub: [add repo link here]  
- LinkedIn: @Pridhvi2297

Enjoy building — and don’t forget to capture a short GIF for LinkedIn! 🚀
