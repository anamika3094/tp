# Adobe India Hackathon 2025 – Challenge 1A:📘 PDF Title & Heading Extractor
Adobe India Hackathon 2025 – Challenge 1A Submission

This project automatically extracts the **document title** and **structured section headings (H1–H4)** from PDF files using a combination of **layout heuristics**, **statistical analysis**, and **semantic scoring**. The output is a clean, hierarchical JSON suitable for indexing, summarization, or Table of Contents (TOC) generation.

---

## 🚀 Features

| Feature                              | Included | Description                                                                 |
|--------------------------------------|----------|-----------------------------------------------------------------------------|
| Title extraction from Page 1         | ✅       | Dynamically detects and merges top font lines                              |
| Heading detection with scoring engine| ✅       | Combines font, alignment, section patterns, and semantic similarity         |
| Semantic filtering with caching      | ✅       | Uses `SentenceTransformer` + `@lru_cache` for speed                         |
| Dynamic layout adaptation            | ✅       | Thresholds adapt per document statistics                                    |
| Heading levels (H1–H6) via regex     | ✅       | Detects structured patterns like 1.2, 1.2.3 etc.                            |
| Table page skipping                  | ✅       | Based on line count and line spacing variance                              |
| Output as JSON file                  | ✅       | Saved in `Output/filename.json` format                                     |
| Batch PDF processing                 | ✅       | Auto-processes all PDFs in `Input/` folder                                 |

---

## 🧠 Strategy Breakdown

### Preprocessing
- Extracts all visible lines with metadata: font, size, bold, italic, x/y position
- Cleans invisible or empty lines

### Page-Level Stats
- Computes average font size, line density, x/y alignment stats
- Identifies and skips probable table-heavy pages

### Title Extraction
- Picks top font lines from Page 1
- Merges multi-line titles using y/x proximity
- Removes repeated headers and footers

### Heading Detection
Each line is scored using:

- Font size ratio
- Bold and italic status
- Section numbering pattern (e.g. 1.2)
- Left alignment proximity
- Semantic similarity (for short lines only)
- Dense region penalty
- Length penalty

Final score determines heading candidacy and assigns H1–H4 level.

### Post-processing
- Deduplicates title/heading overlaps
- Outputs clean, structured JSON

---

## 🧪 Example Output

```json
{
  "title": "Machine Learning Report",
  "headings": [
    {
      "level": "H1",
      "text": "1 Introduction",
      "page": 0
    },
    {
      "level": "H2",
      "text": "1.1 Background",
      "page": 0
    }
  ]
}
```

---

## 🏗️ Folder Structure

```
📁 Input/        → PDF files to process
📁 Output/       → JSON outputs saved here
process_pdfs.py → Main script
README.md       → Project documentation
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
python process_pdfs.py
```

- Automatically processes all `.pdf` files in the `Input/` folder
- Saves structured JSONs to the `Output/` folder

---

## 🛠️ Configuration Highlights

| Setting                      | Value/Logic                              |
|------------------------------|-------------------------------------------|
| Semantic model               | `paraphrase-MiniLM-L6-v2`                |
| Confidence threshold         | 0.7                                      |
| Heading score cap            | 1.5                                      |
| Title duplication check      | Enabled                                  |
| Adaptive thresholds          | Based on per-page stats                  |

---

## ✅ Best Practices

- Place clean, OCR-readable PDFs into the `Input/` folder
- For scanned documents, apply OCR (e.g., using Tesseract) before processing
- Extendable to generate Markdown or HTML TOCs

---

## 🔧 Future Enhancements

- Detect paragraph starts after headings
- Export to Markdown or HTML
- CLI flags for batch and verbosity control
- Add a GUI for drag-and-drop PDF upload
