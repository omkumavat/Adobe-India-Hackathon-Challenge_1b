# Challenge Collections Processor

This Python script extracts relevant sections from PDFs in the `collections/` directory using TF-IDF ranking based on a given persona and job-to-be-done. The results are saved as a structured JSON file for further analysis or downstream tasks.

---

## 📂 Folder Structure

```
.
Challenge_1b/
|
├── Collection1/
    ├── PDFs/
    ├── challenge1b_input.json
    ├── challenge1b_output.json
├── Collection2/
    ├── PDFs/
    ├── challenge1b_input.json
    ├── challenge1b_output.json
├── Collection3/
    ├── PDFs/
    ├── challenge1b_input.json
    ├── challenge1b_output.json
├── challenge_1b.py
├── requirements.txt
├── Dockerfile
README.md
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

## 📤 Output Format (`challenge1b_output.json`)

The output contains:
- Metadata (input details and timestamp)
- Extracted sections with page numbers and importance rank
- Subsection analysis with refined full text

### Example:

```json

{
  "metadata": {
    "input_documents": [
      "South of France - Tips and Tricks.pdf"
    ],
    "persona": "Travel Planner",
    "job_to_be_done": "Plan a trip of 4 days for a group of 10 college friends.",
    "processing_timestamp": "2025-07-28T06:57:45.137424"
  },
  "extracted_sections": [
    {
      "document": "South of France - Tips and Tricks.pdf",
      "section_title": "The Ultimate South of France Travel Companion: Your Comprehensive Guide to Packing,",
      "importance_rank": 1,
      "page_number": 1
    }
  ],
  "subsection_analysis": [
    {
      "document": "South of France - Tips and Tricks.pdf",
      "refined_text": "The Ultimate South of France Travel Companion: Your Comprehensive Guide to Packing, Planning, and Exploring Introduction Planning a trip to the South of France requires thoughtful preparation to ensure a comfortable and enjoyable experience. This guide covers everything from packing essentials to travel tips, catering to all seasons and various activities.",
      "page_number": 1
    }
  ]
}
```

---


## ▶️ How to Run

- ✅ Build the Docker Image: 
```bash
docker build -t pdf-analyzer .
```
- ▶️ Run the Container:
```bash
docker run --rm -v "$(pwd)/Collection_1:/app/Collection_1" pdf-analyzer
```
- This mounts your local Collection_1 folder into the Docker container so the script can read and write data.
- The processed output will be saved to: Collection_1/challenge1b_output.json

The script will:
- Parse the PDFs listed in `challenge1b_input.json` from the `collections/` folder
- Rank and extract relevant sections using TF-IDF
- Generate `challenge1b_output.json` in the same folder as the PDFs

---

## 🧠 How it Works

- **PDF Parsing:** Uses `PyMuPDF` to extract text and font size block-wise.
- **TF-IDF Ranking:** Uses `scikit-learn` to vectorize and rank sections relevant to the persona and task.
- **JSON Output:** Saves a structured output to help personalize learning materials.

---

## ⚙️ Processing Workflow
The script performs the following steps to extract and summarize relevant sections from a collection of PDFs:

1️⃣ Read Input Configuration  
        Loads the persona role, task, and list of PDF filenames from challenge1b_input.json.

2️⃣ Construct Search Query  
  Combines the persona and task into a single query string.
Example:
```bash
"Travel Agent Plan a trip to the South of France"
```

3️⃣ PDF Parsing
For each document listed:

- Opens the PDF using PyMuPDF
- Iterates through each page:
  - Extracts all text spans with font and size
  - Identifies the largest font text as the section heading
  - Captures:
      - title (heading)
      - page_number
      - Full text content of the page

4️⃣ Similarity Matching (TF-IDF)

- Converts all extracted section texts into vectors using TfidfVectorizer
- Transforms the query string into a vector
- Calculates cosine similarity between each section and the query
- Keeps the most relevant section from each document

5️⃣ Top Document Selection

- Sorts the most relevant sections by similarity score
- Selects the top 5 highest-ranking sections

6️⃣ Summarization
For each top-ranked section:

- Cleans the full text (removes noise, symbols, and extra spaces)
- Splits into sentences
- Computes TF-IDF scores for each sentence
- Selects the top 2 most important sentences

7️⃣ Output Generation  
Generates a structured JSON output (challenge1b_output.json) with the following:

- metadata:  
  Contains input document names, persona, task, and processing timestamp

- extracted_sections:  
  List of top documents with:  

  - Section title
  - Page number
  - Importance rank

- subsection_analysis:  
  Contains summarized text (top 2 sentences) from each selected section

---

## ✅ Author

Built by [Balaji Saw , Om Kumavat].

---
