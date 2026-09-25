# Financial Document Intelligence Project

This repository contains a small collection of Python scripts for analyzing financial documents, contracts, and reports. The project combines:

- PDF text extraction and entity parsing
- Named Entity Recognition (NER) training for financial data
- Contract clause extraction using large-language-model style extraction
- Clause-level sentiment analysis for financial documents

The code in this repository is designed as a prototype pipeline for extracting useful information from legal and financial text.

## Project overview

The repository currently includes the following modules:

1. `Langextract.py`
   - Extracts governing-law and contract clauses from a provided text block
   - Uses the `langextract` library with an example-driven prompt
   - Saves the extracted output as a JSONL file and also creates a visual HTML report

2. `test1.py`
   - Reads a PDF file
   - Extracts names, email addresses, and phone numbers
   - Uses `PyPDF2` and spaCy for document parsing and entity extraction

3. `SentimentAnalysis/docling_SA.py`
   - Converts PDF/DOCX files into text and HTML using Docling
   - Runs clause-level sentiment analysis with a Hugging Face model (FinBERT)
   - Produces sentiment summaries and a colored HTML visualization

4. `NER/NER_Training.py`
   - Builds a spaCy training dataset from a JSON annotation file
   - Generates a `.spacy` training file

5. `NER/test_model.py`
   - Loads a trained spaCy NER model
   - Extracts text from PDF/DOCX/TXT inputs
   - Visualizes entities using `displacy`

## Repository structure

```text
entity/
├── Langextract.py
├── test1.py
├── README.md
├── LICENSE
├── financereport.pdf
├── NER/
│   ├── annotations.json
│   ├── config2.cfg
│   ├── NER_Training.py
│   ├── test_model.py
│   ├── training_data2.spacy
│   └── output2/
│       ├── model-best/
│       └── model-last/
├── SentimentAnalysis/
│   ├── docling_SA.py
│   ├── output_html.html
│   └── output_html_sentiment.html
└── test_output/
    └── confidentiality_results.jsonl
```

## Requirements

Use Python 3.10 or newer.

Install the required libraries:

```bash
pip install PyPDF2 spacy langextract docling huggingface_hub
python -m spacy download en_core_web_sm
```

Additional notes:

- `langextract` may require a compatible model backend or API configuration depending on how it is set up in your environment.
- The sentiment analysis script uses the Hugging Face Inference API and expects a valid token.
- The code currently contains several hardcoded Windows file paths; these must be updated before running the scripts in a different environment.

## Setup and usage

### 1) Extract governing law clauses

Run:

```bash
python Langextract.py
```

This script:

- defines an extraction prompt and a few sample examples
- extracts contract clauses from a sample agreement
- writes the result to a JSONL file
- opens a generated HTML visualization in the browser

### 2) Extract contact details from a PDF

Run:

```bash
python test1.py
```

This script expects a PDF file path to be defined in the variable `pdf_path` inside the script. It extracts:

- person names
- email addresses
- phone numbers

### 3) Run sentiment analysis on a DOCX/PDF file

Run:

```bash
python "SentimentAnalysis/docling_SA.py"
```

Before running, make sure:

- the input document path is updated in the script
- `HF_TOKEN` is set as an environment variable, or you replace the placeholder token in `create_hf_client()`

The script creates:

- extracted HTML output
- clause-level sentiment scores
- an aggregated document sentiment label
- a colored sentiment HTML visualization

### 4) Train a spaCy NER model

Run:

```bash
python "NER/NER_Training.py"
```

This script loads annotated data from `NER/annotations.json` and generates a spaCy training dataset at `NER/training_data2.spacy`.

### 5) Test a trained NER model

Run:

```bash
python "NER/test_model.py"
```

This script loads a spaCy model from the output directory and displays entity annotations in a browser-based visualization using `displacy`.

## Important configuration notes

Several scripts use absolute Windows paths, for example:

- `C:\Users\samee\OneDrive\Desktop\NER\financereport.pdf`
- `C:\Users\Ishant Singh\OneDrive\Desktop\new folder\SpringB\...`
- `D:\NER\output2\model-best`

These are example paths and must be changed to match your local machine and project location.

The repository is a research/demo project rather than a production-ready package, and it is best used as a starting point for custom financial-document processing workflows.

## Typical workflow

1. Prepare input PDF/DOCX contracts or financial reports
2. Extract text and metadata from the documents
3. Train or load a financial NER model
4. Extract contract clauses or entity information
5. Run sentiment analysis on document sections
6. Save the results as JSONL or HTML for review

## License

This project is distributed under the MIT license. See the `LICENSE` file for more details.
