# 📄 PDF Contract Parsing CLI


## 🔍 What This Project Does

This project is a **command-line tool** that reads a PDF contract and produces a clean, structured **JSON file**. Instead of scrolling through dozens of pages, you instantly get:

* **Contract Title** (e.g., *Software License Agreement*)
* **Contract Type** (e.g., *Agreement*)
* **Effective Date** (normalized `YYYY-MM-DD`)
* **Sections** (headings detected automatically)
* **Clauses** (bullets, numbering, and inline clauses parsed into items)

It works with **normal text-based PDFs** and also supports **OCR** (scanned documents) when needed.
This makes it useful for **legal tech**, **data analysis**, **AI/NLP preprocessing**, and **compliance automation**.

---

## ⚡ Why It’s Helpful

* **Saves time**: No need to manually extract section/clauses.
* **Machine-readable JSON**: Directly use in downstream apps or ML pipelines.
* **Robust parsing**: Handles numbered sections, bullets (`a)`, `i.`, `1.`), and inline clauses.
* **Graceful fallback**: Works even if libraries are missing (outputs schema-valid JSON).

---

## ⚙️ Setup

### 1. Clone the repo

```bash
git clone https://github.com/<your-username>/<repo-name>.git
cd <repo-name>
```

### 2. Install dependencies

#### System tools (macOS example, via [Homebrew](https://brew.sh)):

```bash
brew install tesseract poppler
```

#### Python libraries:

```bash
pip3 install pymupdf pdfminer.six pillow pytesseract pdf2image python-dateutil
```

---

## ▶️ Usage

Run the parser on any contract PDF:

```bash
python3 mitali_jadhav.py contract.pdf output.json
```

### Options

* `FORCE_OCR=1` → Force OCR even if the PDF already has text.

  ```bash
  FORCE_OCR=1 python3 mitali_jadhav.py scanned.pdf output.json
  ```

---

## ✅ Example

### Input

```bash
python3 mitali_jadhav.py sample_contract.pdf parsed.json
```

### Output (`parsed.json`)

```json
{
  "title": "Software License Agreement",
  "contract_type": "Agreement",
  "effective_date": "2025-01-15",
  "sections": [
    {
      "title": "Definitions",
      "number": "1",
      "clauses": [
        {
          "text": "“Software” means the licensed product provided by Licensor.",
          "label": "a",
          "index": 0
        }
      ]
    }
  ]
}
```

---

## 🧪 How I Tested

* Ran on **8-page**, **25-page**, and **38-page** contracts.
* Verified JSON validity using:

  ```bash
  python3 -m json.tool output.json | head -40
  ```
* All runs completed within ~60s on a **MacBook Air**.

---
