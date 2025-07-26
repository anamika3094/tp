# Adobe India Hackathon 2025 – Challenge 1a: PDF Outline Extractor

This solution automatically extracts a structured **title** and **outline (headings H1–H4)** from academic or technical PDF documents. It combines font-based layout analysis with ML-driven semantic validation to produce clean, hierarchical JSON output.

---

## ✅ Approach Overview

### 1. Text & Style Extraction
- Extracts visible lines using **PyMuPDF**, capturing:
  - `text`, `font size`, `font`, `bold`, `italic`, `x/y` positions, and `color`
- Filters out invisible or empty lines, form fields, and purely decorative text.

### 2. Page-Level Statistical Analysis
Each page is profiled using:
- Font and style distribution
- Header/footer detection using top 2 and bottom 2 lines
- Line spacing variance
- Common x-coordinate positions (for alignment inference)

### 3. Title Detection
- Candidate title lines are chosen from **Page 1** using the top 90% font size.
- Repeated lines from headers/footers are removed.
- Titles across multiple lines are grouped using x/y proximity.
- Final title is selected as the **longest clean block**.

### 4. Heading Extraction (H1–H4)
Each line is scored using both layout and semantic features:

#### 🔢 Scoring Formula:
```
Score = 
  0.25 × Font Score +
  0.2 × Bold Score +
  0.1 × Italic Score +
  0.2 × Section Pattern Match +
  0.15 × Left Alignment Score +
  0.1 × Semantic Similarity Score
  - Dense Line Penalty
  - Length Penalty
```

- **Section Match**: detects patterns like `1.2`, `2.1.1`, etc.
- **Semantic Similarity**: measured against known headings like `introduction`, `summary`, `results`, using:

```python
SentenceTransformer("paraphrase-MiniLM-L6-v2")
```

- Headings are assigned `H1–H4` based on the depth of section numbering or layout importance.

### 5. Table Detection
- Pages with **>40 lines** and **low line-spacing variance (<5)** are flagged as **table-heavy** to avoid false headings.

### 6. Caching for Speed
- Semantic similarity uses `@lru_cache` to minimize redundant model inference and accelerate large PDF processing.

---

## 🧰 Models & Libraries Used

| Purpose            | Library / Model                            |
|--------------------|---------------------------------------------|
| PDF Parsing        | `PyMuPDF` (`fitz`)                          |
| Semantic Matching  | `sentence-transformers` (MiniLM-L6-v2)      |
| Data Analysis      | `collections`, `re`, `unicodedata`, `numpy` |

---

## 🏗️ Project Structure

```
📁 Input        → PDF files for processing  
📁 Output       → JSON output files  
process_pdfs.py → Main script  
README.md       → Documentation  
```

---

## ⚙️ Build & Run Instructions

> 📌 This is for documentation only. Actual execution should use the "Expected Execution" flow.

### Install Dependencies

```bash
pip install pymupdf
pip install sentence-transformers
```

### Run Script

```bash
python process_pdfs.py
```

- The script processes all PDFs in the `Input/` folder and saves structured output to `Output/`.

---

## 🔍 Highlights

- Multi-line title detection and merging using layout grouping
- Smart filtering of headers, footers, form fields, and S.No patterns
- Clean semantic heading validation using pre-trained embeddings
- Scalable architecture for large PDF documents
