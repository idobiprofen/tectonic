# SD Worx — Trusted Knowledge Hub

> **Tectonic Hackathon 2026 Submission for SD Worx Challenge: "Unlock the Knowledge Within"**

## Project Overview & Hackathon Pitch

> "To ensure organizational documents are completely trustworthy and to detect conflicting or outdated information automatically, we created a centralized Knowledge Hub backed by AI Pre-Auditing and Grounded RAG. 
> 
> With every ingested document, metadata including the author, title, category, and region is captured alongside an automated timestamp. The document is then evaluated by our AI pre-audit engine, which cross-references document authors against an accredited author whitelist (`accredited_authors.txt`) and scans for rate discrepancies, version overlaps, or unverified draft tags to compute an AI Confidence Score.
> 
> If the confidence score meets our high-trust threshold (>= 0.80) with an accredited author, the document can be automatically indexed. Otherwise, it is routed to a **Side-by-Side Human Checkpoint** where domain experts read document text side-by-side with AI audit flags to approve, supersede, or reject files. 
> 
> For search and Q&A, queries and document passages are embedded into vectors using cosine similarity ranking to retrieve top grounded context chunks. This ensures rapid answers while eliminating LLM hallucinations. If a query pertains to undocumented procedures or low vector similarity, the system automatically escalates the question to a credited domain expert with direct Teams and email contacts. 
> 
> Our solution transforms fragmented organizational files into a trusted shared resource—saving SD Worx employees valuable time while guaranteeing answer credibility."

---

## 4-Tab Streamlit Application Structure

- **1. Document Upload & Ingestion**:
  - File upload form (PDF, TXT, Markdown) & raw text paste.
  - Automatic directory scanner for `pending_documents/` folder.
  - Batch Controls: Import to queue, AI Auto-Approve scan (Score >= 0.80), Reset Environment.

- **2. Side-by-Side Verification Checkpoint**:
  - Left Column (Document Reader): Full document viewer, Accredited Author Whitelist badge, AI audit flags, AI summary.
  - Right Column (Action Panel): Select credited owner, add verification notes, Approve / Approve & Supersede / Reject buttons.

- **3. Credited Database (Knowledge Vault)**:
  - Metric summary cards (Total Docs, Approved & Credited, Pending Checkpoint, Superseded/Archival).
  - Filterable document registry table with status badges, owner attribution, and full content viewer.

- **4. Trust-Aware Chatbot & Expert Routing**:
  - Grounded RAG Q&A interface with preset sample queries.
  - Passage chunk visualizer displaying TF-IDF vector similarity scores and source citations.
  - Smart Human Escalation card for low-confidence queries routing directly to domain specialists.

---

## Accredited Authors Whitelist (`accredited_authors.txt`)

The system verifies document authors against `accredited_authors.txt`:
```
Jane Doe
John Smith
Alex Taylor
Chris Jordan
Morgan Lee
```
- **Match**: Grants trust bonus (`[VERIFIED] Accredited Author Whitelist Match`).
- **No Match**: Flags warning (`[WARNING] Unaccredited Author Notice`) and reduces initial AI confidence score.

---

## Quick Start & Setup

### Prerequisites
- Python 3.10+

### Step 1: Install Dependencies
```bash
pip install streamlit pypdf google-genai pandas
```

### Step 2: Run the Application
```bash
streamlit run app.py
```
Open your browser at `http://localhost:8501`.

---

## Directory & File Structure

```
tectonic/
├── app.py                     # Main 4-Tab Streamlit Web Application
├── ai_engine.py               # AI Pre-Audit, Author Whitelist Check & Grounded RAG Pipeline
├── database.py                # SQLite Storage, Seeder & Reset Demo Logic
├── accredited_authors.txt     # Accredited Author Whitelist File
├── pending_documents/         # Folder Queue for incoming unverified files
├── approved_documents/        # Directory for expert-verified & indexed files
├── rejected_documents/        # Directory for rejected files
└── README.md                  # Project Documentation
```

---

## Security & Governance
- **Input Sanitization**: File uploads are pre-scanned and validated.
- **SQL Parameterization**: Parameterized queries across SQLite operations to prevent injection risks.
- **Audit Lineage**: Every document records credited owners, expert verification notes, and timestamps.
