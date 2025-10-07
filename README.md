# Big Data – Stage 1: Search Engine Project

**University:** ULPGC – GCID  
**Academic Year:** 2024/2025  
**Group:** BD Power  

**Members:**  
- Jaime Rivero Santana  
- Gabriel Munteanu Cabrera  
- Andrea Dumpiérrez Medina  
- Agustín Darío Casebonne  

**Repository:** [https://github.com/Gabrii8/Big-Data](https://github.com/Gabrii8/Big-Data)

---

## Abstract

This project corresponds to **Stage 1** of the Big Data course at ULPGC.  
It aims to build the **data ingestion and storage layer** of a text-based search engine.  
The system downloads real books from **Project Gutenberg**, cleans and structures them into a **Data Lake**, and creates a **Data Mart** containing **inverted indexes** to support future search functionality.  
Two datalake strategies and two index storage formats were implemented and compared in terms of **organization**, **performance**, and **readability**.

---

## 1. Project Overview

The objective of this first stage is to design a **scalable data layer** that allows:
- Automatic book downloading and control of processed files.  
- Structured and auditable data storage through different Data Lake organizations.  
- Building and saving **inverted indexes** for keyword-based retrieval.  
- Comparing **storage formats** (JSON vs TXT) and **data organizations** (by date/hour vs by author).  
- Evaluating **efficiency** and **usability** through performance metrics and visualizations.

---

## 2. System Architecture

The overall pipeline can be summarized as follows:
Project Gutenberg → Crawler → Control Layer → Data Lake → Data Mart (Inverted Index) → Visualization


### Components

#### 2.1. Crawler  
Responsible for automatically downloading books from Project Gutenberg using their public IDs.  
- Downloads files from URLs such as `https://www.gutenberg.org/cache/epub/{id}/pg{id}.txt`.  
- Detects markers to split each file into **header** (metadata) and **body** (text).  
- Handles exceptions and network errors (404, timeouts).  

#### 2.2. Control Layer  
Keeps track of all processed books to avoid duplicates.  
- Files created:  
  - `control_log.csv` → contains book ID, file paths, and status.  
  - `downloaded_books.txt` → stores already downloaded book IDs.  
- Functions:  
  - `already_downloaded(book_id)`  
  - `register_in_control(book_id, header_path, body_path, state="OK")`

#### 2.3. Data Lake  
Stores downloaded data in two different organizational strategies:
- **Version A – By Date and Hour:**  

datalake/YYYYMMDD/HH/{id}.header.txt | {id}.body.txt
- **Version B – By Author Initial (Alphabetical):**  


datalake_alpha/A/{id}.header.txt | {id}.body.txt
The author’s name is extracted from the header using a regex search (`Author:`).  
Unknown or non-alphabetical initials are stored under `Other`.

#### 2.4. Data Mart  
Processes the books from the Data Lake and builds an **inverted index** mapping each unique word to the list of books where it appears.  
Two output formats were compared:
- **JSON:** structured and easy to load for future processing.  
- **TXT:** plain text format for human readability.  

#### 2.5. Visualization  
Performance comparisons and statistics are displayed using **matplotlib** charts.

---

## 3. Functionalities Implemented

- Automatic book download and storage.  
- Header/body separation using Gutenberg markers.  
- Duplicate control and logging.  
- Two Data Lake organizations (temporal and alphabetical).  
- Inverted index construction in JSON and TXT formats.  
- Comparison between storage strategies and visualization of execution times.  
- Scalability up to hundreds of books.

---

## 4. Dependencies and Environment Setup

Before running the notebook, ensure you have **Python 3.11** or higher and install the required libraries:

---


 ## 5. Setup and Execution Instructions
 ### Step 1: Clone the Repository
git clone https://github.com/Gabrii8/Big-Data.git
cd Big-Data/proyecto_stage1

### Step 2: Open the Project

Open the notebook code.ipynb in Visual Studio Code (Jupyter extension) or Jupyter Notebook.

### Step 3: Run the Notebook Cells Sequentially

Import dependencies and helper functions.

Execute download_book() → Creates Data Lake (by date/hour).

Execute download_book_alpha() → Creates Data Lake (by author initial).

Execute build_inverted_index() → Builds inverted indexes in JSON and TXT.

Execute compare_datalakes() → Compares both Data Lake strategies.

(Optional) Run the loop for up to 300 books for scalability tests.

### Step 4: Visualize the Results

The notebook automatically produces:

Figure 1: Inverted Index (JSON vs TXT) performance.

Figure 2: Data Lake (Date/Hour vs Author) comparison.

Figure 3: Metadata benchmark (SQLite vs TinyDB).


---

## 6. Repository Structure
control/
  ├── control_log.csv
  └── downloaded_books.txt

datalake/
  └── YYYYMMDD/HH/{id}.header.txt | {id}.body.txt

datalake_alpha/
  └── A-Z/{id}.header.txt | {id}.body.txt

datamarts/
  ├── inverted_index.json
  └── inverted_index.txt

proyecto_stage1/
  └── code.ipynb

---

## 7. Data Lake Strategies Comparison
Description

Two organizational strategies were implemented for the Data Lake:

Strategy	Folder Example	Advantages	Disadvantages
Date/Hour	datalake/20251005/19/	Easy to audit, chronological control	Low semantic meaning for exploration
Author (A-Z)	datalake_alpha/A/	Intuitive to browse by content	More I/O operations (slower creation)
Results
Strategy	Folders	Files	Time (s)
Date/Hour	3	12	7.611
Author	7	14	45.224

Conclusion:
While the alphabetical organization is slower due to additional directory operations, it provides a clearer and more intuitive structure for human inspection and presentation. The temporal strategy remains ideal for batch automation and logging.

---

## 8. Inverted Index (Data Mart) Comparison

Two storage formats were tested to store the inverted index:

Format	Description	Time (s)	Notes
JSON	Structured format, suitable for data interchange	1.1468	Easier to reload and integrate
TXT	Plain text representation	0.6323	Slightly faster to write

Conclusion:
Both formats achieve comparable performance.
TXT is marginally faster, but JSON offers significant benefits for scalability, validation, and machine readability.

---

## 9. Metadata Storage Benchmark

An additional test compared lightweight database solutions for storing metadata.

System	Insert (s)	Query (s)	Result Count
SQLite	0.0113	0.0009	1
TinyDB	0.0019	0.0017	1

Conclusion:
SQLite is more suitable for structured data and complex queries, while TinyDB is extremely lightweight for prototypes and small datasets.

---

## 10. Scalability

The system is designed to handle large-scale downloads using a simple loop:

for book_id in range(1, 301):
    try:
        download_book_alpha(book_id)
    except Exception as e:
        print(f"[ERROR] Book {book_id}: {e}")


This structure allows scaling beyond 300 books without code modifications.
The control layer prevents re-downloads by checking downloaded_books.txt.

---

## 11. Design Decisions Summary
Component	Decision	Rationale
Crawler	Uses requests library and regex-based parsing	Reliable and simple for plain text sources
Control Layer	Maintains CSV and TXT logs	Ensures reproducibility and avoids duplicates
Data Lake	Dual structure (temporal + alphabetical)	Supports both auditing and usability
Data Mart	JSON as main output, TXT as alternative	JSON enables structured reloading
Metadata DB	SQLite selected for scalability	Prepares for future indexing and queries
Visualization	matplotlib	Standard library for Python-based analysis

---

## 12. Discussion

Performance:
Execution times are consistent with expectations for a sequential pipeline using Python I/O.
The difference between datalake strategies stems mainly from directory creation and author extraction overhead.

Scalability:
The system supports parallelization and can easily be extended for multi-threaded downloads in Stage 2.

Usability:
The alphabetical organization is beneficial for demonstration and exploratory purposes, making the project clearer to review.

Reproducibility:
Each component can be re-executed independently thanks to the control and logging system.

---

## 13. Future Work

Text preprocessing: implement token normalization, stopword removal, and stemming/lemmatization.

Query engine: support Boolean queries (AND, OR, NOT) and TF-IDF ranking.

Parallelization: concurrent downloads and index construction.

Metadata database: integrate relational or NoSQL solutions (SQLite, MongoDB).

Web interface: simple front-end for search and visualization.

---

## 14. Credits

Text Data: Project Gutenberg

Language: Python 3.11

Libraries Used: requests, pathlib, collections, json, matplotlib, re, time

Development Environment: Visual Studio Code (Jupyter Notebook Extension)

---

## 15. Deliverables

This repository includes:

code.ipynb → complete notebook with all code.

control/, datalake/, datalake_alpha/, datamarts/ → generated data folders.

Stage1_Report_BD_Power.pdf → official Stage 1 report (in /docs/).

README.md → current documentation file.
