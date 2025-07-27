# Adobe India Hackathon 2025 – 🔍 Challenge 1B: Multi-Collection PDF Analysis
**Adobe India Hackathon 2025 – Challengen 1B Submission**

Welcome to the second phase of reimagining PDF reading! This challenge focuses on **intelligent content extraction**, **contextual analysis**, and **semantic matching** across multiple documents to serve **persona-based needs**.

---

## 🌟 Objective

Given multiple folders of PDF files and a `persona` with a specific `job_to_be_done`, extract the **most relevant sections** across the PDFs and rank them by semantic importance using sentence embeddings.

---

## 🧠 Core Approach

1. **Input Structure**:
   - Each folder (`Collection 1`, `Collection 2`, etc.) contains:
     - `input.json`: Lists PDFs, persona, and job description
     - PDF files for analysis

2. **Extraction**:
   - Uses `PyMuPDF` to parse each PDF and extract section-like headings based on formatting cues (e.g., title-cased lines under 100 chars).

3. **Semantic Matching**:
   - Loads the `all-MiniLM-L6-v2` model from `sentence-transformers`
   - Embeds the `persona + job_to_be_done` as **context**
   - Each section's title is compared using `cosine similarity`

4. **Ranking**:
   - Sections are scored and sorted by relevance
   - Top 10 sections are saved
   - Top 5 go into `subsection_analysis` for fine-grained insights

5. **Output**:
   - Results stored in `output.json` inside each collection

---

## 🧪 Sample Output Format

```json
{
  "metadata": {
    "input_documents": ["doc1.pdf", "doc2.pdf"],
    "persona": "Research Analyst",
    "job_to_be_done": "Understand recent trends in GenAI",
    "generated_on": "2025-07-26T14:00:00Z"
  },
  "extracted_sections": [
    {
      "section_title": "GenAI in Enterprise Applications",
      "page_number": 3,
      "document": "genai_trends.pdf",
      "importance_rank": 1
    }
  ],
  "subsection_analysis": [
    {
      "document": "genai_trends.pdf",
      "page_number": 3,
      "refined_text": "GenAI in Enterprise Applications"
    }
  ]
}
```

---

## 📂 Folder Structure

```
📁 Challenge_1b/
├── Collection 1/
│   ├── input.json
│   ├── *.pdf
│   └── output.json (auto-generated)
├── Collection 2/
│   └── ...
├── Collection 3/
│   └── ...
├── analyze_collections.py
└── README.md
```

---

## ⚙️ Setup Instructions

```bash
pip install pymupdf
pip install sentence-transformers
```

---

## ▶️ How to Run

Simply execute the script:

```bash
python analyze_collections.py
```

- Processes all collections (`Collection 1`, `Collection 2`, etc.)
- Outputs are saved to each respective folder

---

## 🛠️ Highlights

| Capability                    | Description |
|------------------------------|-------------|
| Multi-PDF comparison         | Extracts insights from many documents simultaneously |
| Persona-specific relevance   | Uses semantic embedding to tailor output |
| Top-k importance ranking     | Sections are ranked by cosine similarity |
| Scalable design              | Easily extendable to more folders and use-cases |
| Lightweight, fast & accurate | Uses a lightweight transformer for performance |

---

## 🔮 Future Enhancements

- Enable section chaining for topic continuity
- Embed full paragraphs for deeper context
- GUI or notebook interface for visualization
- Integration with Adobe Embed API (Phase 2)
