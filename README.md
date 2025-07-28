# Challenge Collections Processor

This Python script extracts relevant sections from PDFs in the `collections/` directory using TF-IDF ranking based on a given persona and job-to-be-done. The results are saved as a structured JSON file for further analysis or downstream tasks.

---

## 📂 Folder Structure

```
.
Collection_3/
│
├── challenge1b_input.json
├── challenge1b_output.json
├── challenge_1b.py
├── requirements.txt
└── README.md
```

---

## 🔧 Requirements

Install the required Python packages:

```bash
pip install -r requirements.txt
```

### `requirements.txt`

```
PyMuPDF
scikit-learn
```

---

## 📥 Input File Format (`input.json`)

This JSON file should describe:
- Which PDF files to process
- Who the persona is
- What the persona needs to do (task)

### Example:

```json
{
  "documents": [
    {
      "filename": "example.pdf"
    }
  ],
  "persona": {
    "role": "Engineering Student"
  },
  "job_to_be_done": {
    "task": "Understand electrical experiment procedures"
  }
}
```

---

## ▶️ How to Run

```bash
python process_collections.py
```

The script will:
- Parse the PDFs listed in `input.json` from the `collections/` folder
- Rank and extract relevant sections using TF-IDF
- Generate `challenge_output.json` in the same folder as the PDFs

---

## 📤 Output Format (`challenge_output.json`)

The output contains:
- Metadata (input details and timestamp)
- Extracted sections with page numbers and importance rank
- Subsection analysis with refined full text

### Example:

```json
{
  "metadata": {
    "input_documents": ["example.pdf"],
    "persona": "Engineering Student",
    "job_to_be_done": "Understand electrical experiment procedures",
    "processing_timestamp": "2025-07-28T12:30:00"
  },
  "extracted_sections": [
    {
      "document": "example.pdf",
      "section_title": "Introduction to Inverters...",
      "importance_rank": 1,
      "page_number": 2
    }
  ],
  "subsection_analysis": [
    {
      "document": "example.pdf",
      "refined_text": "Inverter is a power electronic device that...",
      "page_number": 2
    }
  ]
}
```

---

## 🧠 How it Works

- **PDF Parsing:** Uses `PyMuPDF` to extract text and font size block-wise.
- **TF-IDF Ranking:** Uses `scikit-learn` to vectorize and rank sections relevant to the persona and task.
- **JSON Output:** Saves a structured output to help personalize learning materials.

---

## ✅ Author

Built by [Balaji Saw , Om Kumavat].

---

## 📄 License

MIT License

