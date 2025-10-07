# Big-Data

## Stage 1: Search Engine Project

### University
**ULPGC – GCID (Ciencias e Ingeniería de Datos)**  
**Academic Year:** 2024 / 2025  
**Course:** Big Data  

### Group Information
**Group Name:** BD Power  

**Members:**  
- Jaime Rivero Santana  
- Gabriel Munteanu Cabrera  
- Andrea Dumpiérrez Medina  
- Agustín Darío Casebonne  

**Repository URL:** [https://github.com/BD-Power/stage_1](https://github.com/BD-Power/stage_1)

---

## 1. Introduction and Objectives

This project corresponds to Stage 1 of the *Big Data* practical assignment.  
The objective is to design and implement the **data ingestion and storage layer** of a text-based search engine using real data from **Project Gutenberg**.  

The system must:
- Download multiple books from the Gutenberg repository.  
- Store them in a **Data Lake** organized in two different ways (by date/hour and by author initial).  
- Build a **Data Mart** with inverted indexes in JSON and TXT formats.  
- Perform comparisons of performance and storage efficiency between both strategies.  
- Generate visualizations and benchmarks for analysis.

---

## 2. System Architecture

The pipeline implemented in this project consists of the following components:

1. **Data Source:**  
   Books are obtained from [Project Gutenberg](https://www.gutenberg.org), containing metadata, headers, and body text.

2. **Crawler and Downloader:**  
   A function automatically downloads books from Gutenberg using their ID and extracts text between Gutenberg markers.

3. **Control Layer:**  
   Manages logs, download tracking, and error handling.  
   Files:  
   - `control/control_log.csv`  
   - `control/downloaded_books.txt`

4. **Data Lake:**  
   Raw text storage with two organizational strategies:
   - **By date and hour** → `/datalake/YYYYMMDD/HH/`
   - **By author initial** → `/datalake_alpha/A-Z/`

5. **Data Mart (Inverted Index):**  
   Builds a word–book mapping in both **JSON** and **TXT** formats for search efficiency.

6. **Metadata Management:**  
   Two lightweight databases (**SQLite** and **TinyDB**) are compared for performance during insertion and queries.

7. **Visualization Layer:**  
   Performance charts are generated using **Matplotlib** for comparisons between strategies.

---

## 3. Functionalities Implemented

- Automatic downloading of books using Gutenberg IDs.  
- Header and body text separation using Gutenberg markers.  
- Duplicate download prevention with logging and control.  
- Two Data Lake organization strategies:  
  - **Temporal (by date/hour)**  
  - **Alphabetical (by author initial)**  
- Inverted index creation in **JSON** and **TXT** formats.  
- Comparison between Data Lake organizations and Data Mart formats.  
- Benchmarking of **SQLite** vs **TinyDB** for metadata.  
- Visualization of execution times and storage performance.  
- Scalable design — can handle up to hundreds of books.  

---

## 4. Dependencies and Environment Setup

Before running the notebook, make sure you have **Python 3.11+** and install the required libraries


---

## 5. Setup and Execution Instructions
#### Step 1: Clone the Repository
git clone https://github.com/BD-Power/stage_1.git
cd stage_1

#### Step 2: Open the Notebook

Open the file code.ipynb using VS Code (Jupyter extension) or Jupyter Notebook.

#### Step 3: Run the Notebook Cells Sequentially

Import dependencies and helper functions.

Run the following functions in order:

download_book() → Creates the Data Lake (by date/hour)

download_book_alpha() → Creates the Data Lake (by author initial)

build_inverted_index() → Builds the inverted indexes (JSON and TXT)

compare_datalakes() → Compares both Data Lake strategies

(Optional): Run the scalability test loop for up to 300 books.

#### Step 4: Visualize the Results

The notebook automatically produces performance visualizations:

Figure 1: Inverted Index (JSON vs TXT)

Figure 2: Data Lake (Date/Hour vs Author)

Figure 3: Metadata benchmark (SQLite vs TinyDB)

---

## 6. Repository Structure
control/
 ├── control_log.csv
 └── downloaded_books.txt

datalake/
 └── YYYYMMDD/HH/{id}.header.txt
 └── YYYYMMDD/HH/{id}.body.txt

datalake_alpha/
 └── A-Z/{id}.header.txt
 └── A-Z/{id}.body.txt

datamarts/
 ├── inverted_index.json
 └── inverted_index.txt

proyecto_stage1/
 └── code.ipynb

---
## 7. Data Lake Comparison

Two organizational strategies were implemented:

Strategy	Structure Example	Folders	Files	Time (s)
Date/Hour	datalake/20251005/20/	3	12	7.611
Author (A–Z)	datalake_alpha/A/	7	14	45.224

🔹 The Date/Hour organization is faster and ideal for automation pipelines.

🔹 The Alphabetical organization is more intuitive for browsing, though slower due to folder operations.

---

## 8. Inverted Index Comparison (Data Mart)
Format	Description	Time (s)
JSON	Structured and easily loadable for data interchange	1.1468
TXT	Lightweight and faster to read/write	0.6323

 TXT performs faster due to simpler structure,
while JSON is more suitable for structured storage and scalability.

---
## 9. Metadata Benchmark
Database	Insert (s)	Query (s)	Results
SQLite	0.0113	0.0009	1
TinyDB	0.0019	0.0017	1

 SQLite provides better performance for larger and structured data,
while TinyDB is lightweight and ideal for quick testing or small-scale environments.

---
## 10. Design Decisions Summary
Component	Decision	Reason
Crawler	requests + regex	Efficiently processes Gutenberg plain-text books
Control Layer	CSV and TXT logs	Guarantees reproducibility and monitoring
Data Lake	Two storage modes (date/hour, author)	Combines automation and human readability
Data Mart	JSON and TXT formats	Enables comparison between structure and speed
Metadata	SQLite + TinyDB	Demonstrates relational vs document-based storage
Visualization	matplotlib	Clear and simple performance representation

---

## 11. Discussion

The alphabetical Data Lake improves navigation but increases processing time due to directory operations.

The temporal Data Lake is faster and better for automated ingestion tasks.

The TXT inverted index provides higher writing speed, while JSON facilitates structured querying.

SQLite outperforms TinyDB for large datasets and structured metadata.

---
## 12. Future Improvements

 - Implement text preprocessing (stopword removal, stemming, lemmatization).

 - Add Boolean and ranked queries (TF-IDF, BM25).

- Parallelize downloading and indexing to improve performance.

 - Integrate persistent NoSQL databases (e.g., MongoDB).

 - Create a web-based interface for book search and visualization.

---
## 13. Credits

Data Source: Project Gutenberg
Programming Language: Python 3.11
Libraries Used: requests, pathlib, json, collections, matplotlib, re, time, tinydb, sqlite3
Development Environment: Visual Studio Code with Jupyter extension

---
## 14. Deliverables

code.ipynb → Main notebook.

control/, datalake/, datalake_alpha/, datamarts/ → Data layers.

Stage1_Report_BD_Power.pdf → Final written report.

README.md → This documentation file.

