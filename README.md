# PDF Data Extractor

This repository contains a robust Python script designed to intelligently extract and scale numerical data from PDF documents. It processes both general text content and structured tables, then **identifies the single largest numerical value** within the entire document after applying appropriate contextual scaling.

## How It Works

The script operates through a methodical pipeline:
1.  **PDF Parsing:** It simultaneously extracts structured tables using `pdfplumber` and all plain text content using `PyMuPDF`.
2.  **Number Identification:** It systematically scans all extracted text and table cells for numerical values, handling various formats (e.g., commas, currency symbols, negative numbers).
3.  **Contextual Scaling:** For each identified number, it applies a sophisticated logic to infer its true scale (e.g., thousands, millions, billions) by analyzing nearby keywords in column headers, surrounding text, the current page, and document-wide disclaimers.
4.  **Largest Value Determination:** Finally, from all the processed and scaled numbers, the script determines and reports the single greatest numerical value found across the entire document, along with its page location.

## Project Pipeline

The script processes PDF documents through a clear, multi-stage pipeline as visualized below:

```mermaid
graph TD
    A[Start] --> B(PDF File Input);

    B --> C{Orchestrator: main() function};

    C --> D1[Step 1: Extract Tables with pdfplumber]:::process;
    C --> D2[Step 2: Extract All Text with PyMuPDF]:::process;

    D1 --> E{Tables as DataFrames};
    D2 --> F{Text Page by Page};

    E & F --> G{Combined Data: Tables & Text};

    G --> H[Step 3: Find Numbers & Apply Scales]:::process;

    H --> I{For Each Number Found};
    I --> J[Clean & Convert Number]:::sub_process;
    I --> K[Infer Scale from Context]:::sub_process;

    K --> L{Scale Hierarchy Applied};
    L --> M[Store Scaled Number & Page];

    M --> N[All Scaled Numbers Collected];

    N --> O[Identify Largest Scaled Number];

    O --> P[Final Report: Largest Value & Page];
    P --> Q[End];

    classDef process fill:#a2d2ff,stroke:#007bff,stroke-width:2px,color:#333;
    classDef sub_process fill:#d4f0ff,stroke:#007bff,stroke-dasharray: 5 5,color:#333;


## Setup and Usage
To get this script up and running, follow these steps:

1.  **Save the script:** Save the provided Python code as a `.py` file (e.g., `pdf_parser.py`) on your local machine.
2.  **Install Dependencies:** Open your terminal or command prompt and install the necessary Python libraries using `pip`:
    ```bash
    pip install pdfplumber pymupdf pandas
    ```
    * `pdfplumber`: For robust table extraction.
    * `pymupdf` (fitz): For efficient text extraction page by page.
    * `pandas`: For handling tabular data (DataFrames) extracted from PDFs.
3.  **Place your PDF:** Ensure the PDF document you wish to parse is accessible from where you run the script. It's recommended to place it in the same directory as `pdf_parser.py`, or provide its full path.
4.  **Run the Code:** Navigate to the directory where you saved `pdf_parser.py` in your terminal, and execute the script by providing the PDF file path as a command-line argument:
    ```bash
    python pdf_parser.py "path/to/your/document.pdf"
    ```
    * **Example:** `python pdf_parser.py "my_financial_report.pdf"`
    * **Note:** Use double quotes around the PDF path if it contains spaces.
