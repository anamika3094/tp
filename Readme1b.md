# Adobe India Hackathon 2025 – 🔍 Challenge 1B: Multi-Collection PDF Analysis
**Adobe India Hackathon 2025 – Challengen 1B Submission**

This project performs **persona-aware PDF section extraction** across multiple collections. It reads each document, extracts likely section headers, computes **semantic similarity** with a provided persona and job-to-be-done context, and ranks the top relevant segments. Output is a ranked, structured JSON for insight surfacing or summarization.

---

## 🚀 Features

| Feature                          | Included | Description                                                                 |
|----------------------------------|----------|-----------------------------------------------------------------------------|
| Multi-document collection input  | ✅       | Handles collections like `Collection 1`, `Collection 2`, etc.               |
| Persona + Job context embedding  | ✅       | Embeds both role and job for semantic relevance                            |
| Section title detection          | ✅       | Extracts title-cased, short lines from each page                           |
| Semantic similarity ranking      | ✅       | Cosine similarity using `SentenceTransformer` embeddings                    |
| Importance scoring & ranking     | ✅       | Ranks and selects most relevant sections                                   |
| Subsection analysis              | ✅       | Includes refined insights from top-ranked sections                         |
| JSON Output per collection       | ✅       | Outputs `output.json` for each document collection                         |
| Batch processing for collections | ✅       | Auto-processes each collection folder                                      |

---

## 🧠 Strategy Breakdown

### Input Format

Each collection contains:
- `input.json`:
  - `persona` → role name (e.g., “Research Analyst”)
  - `job_to_be_done` → task (e.g., “Understand AI in Education”)
  - List of PDF filenames to scan

### Preprocessing

- Reads line-level text from all PDFs
- Filters candidate section titles based on casing, length, and structure

### Context Embedding

- Converts `persona` + `job_to_be_done` into a unified sentence
- Embeds using `all-MiniLM-L6-v2` from `sentence-transformers`

### Semantic Scoring

Each candidate section title is scored based on cosine similarity with context.
Top N (e.g. 10) sections are retained.

### Output

Creates a structured output JSON:
- `extracted_sections`: Top 10 ranked segments with metadata
- `subsection_analysis`: Top 5 lines shown again as refined insights

---

## 🧪 Example Output

```json
{
  "metadata": {
    "persona": "Researcher",
    "job_to_be_done": "Understand GenAI trends"
  },
  "extracted_sections": [
    {
      "section_title": "GenAI in Healthcare",
      "document": "ai_trends.pdf",
      "page_number": 3,
      "importance_rank": 1
    }
  ],
  "subsection_analysis": [
    {
      "document": "ai_trends.pdf",
      "page_number": 3,
      "refined_text": "GenAI in Healthcare"
    }
  ]
}
```

---

## 🏗️ Folder Structure

```
📁 Collection 1/     → Contains PDFs, input.json, output.json
📁 Collection 2/     → Additional collection (optional)
analyze_collections.py → Main script
README.md           → Project documentation
```

---

## ⚙️ Build & Run Instructions

> This section is for documentation purposes only.

### Prerequisites

- Python 3.8+
- Install dependencies:

```bash
pip install pymupdf
pip install sentence-transformers
```

### Run

```bash
python analyze_collections.py
```

- Automatically processes collections like `Collection 1`, `Collection 2`, etc.
- Saves `output.json` for each with ranked results

---

## 🛠️ Configuration Highlights

| Setting                  | Value/Logic                        |
|--------------------------|-------------------------------------|
| Semantic model           | `all-MiniLM-L6-v2`                 |
| Ranking logic            | Cosine similarity from embeddings  |
| Subsection sample count  | Top 5 from final rankings           |
| Missing PDF check        | Skips files not found in folder     |

---

## ✅ Best Practices

- Use clearly structured PDFs (non-scanned)
- Ensure section headers are reasonably title-cased and meaningful
- Extendable to fetch paragraph text around section headers

---

## 🔧 Future Enhancements

- Paragraph context extraction after headings
- Cross-document linking between sections
- Support for overlapping personas and multitask jobs
- GUI or dashboard for insights visualization
