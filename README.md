# 📧 Automated Sales KPI Alert System

## 📌 Project Overview
**Goal:** Eliminate manual monitoring of sales performance by automating daily KPI checks and notifications.

In a fast-paced retail environment, Sales Managers often spend valuable time manually reviewing daily Excel logs to identify underperforming agents. This project utilizes **Alteryx** to ingest daily sales data, apply business logic to identify missed targets, and automatically trigger personalized email alerts to specific agents (cc'ing management).

## 🛠 Tools & Technologies
*   **Alteryx Designer** (ETL, Logic, Reporting)
*   **Tools Used:** Input Data, Filter, Formula, DateTime Functions, Report Text, Email Tool.
*   **Data Source:** Daily CSV/Excel Logs.

## ⚙️ Workflow Logic

The workflow allows for "Touchless Reporting" by following this logic pipeline:

1.  **Dynamic Ingestion:**
    *   Reads the master `Daily_Sales.csv` file.
    *   **Date Filtering:** Uses `DateTimeToday()` to ensure the workflow *only* processes data relevant to the current date (ignoring historical or future records).
    *   *Formula:* `[Date] = DateTimeFormat(DateTimeToday(),"%Y-%m-%d")`

2.  **Performance Flagging:**
    *   Calculates variance between `[Daily_Sales]` and `[Target]`.
    *   Filters data to isolate only agents where `Sales < Target`.

3.  **Dynamic Message Generation:**
    *   **Subject Line:** Dynamically created via Formula Tool (e.g., "Sales Missed for Date: 2025-12-23").
    *   **Body Content:** Uses the **Report Text Tool** to inject row-level data into an HTML template.
    *   *Template:* "Hello [Agent], your sales of $[Sales] were below target..."

4.  **Automated Distribution:**
    *   Routes the generated messages to the **Email Tool**.
    *   Sends unique emails to the `Agent_Email` column and cc's the `Manager_Email`.

## 📸 Workflow Visualization

<img width="808" height="1362" alt="image" src="https://github.com/user-attachments/assets/a978c3c2-7a0a-49d1-b89f-a2bc3aebec95" />

*(Screenshot of the full pipeline)*

<img width="808" height="1362" alt="image" src="https://github.com/user-attachments/assets/821e8d8d-9d13-4fe1-9c8a-b707c8ad60e8" />

*(Detail of how dynamic fields are injected into the email body)*

## 🚀 How to Run This Project

1.  **Prerequisites:** You need Alteryx Designer installed.
2.  **Data Setup:**
    *   The workflow looks for `Daily_Sales.csv`.
    *   Ensure the Date format in the CSV is `YYYY-MM-DD`.
3.  **SMTP Configuration:**
    *   Open the **Email Tool** at the end of the workflow.
    *   Enter your SMTP Host (e.g., `smtp.gmail.com`).
    *   Enter your credentials (Username/App Password).
    *   *Note: Credentials have been removed from this repo for security.*
4.  **Scheduling:**
    *   This workflow is designed to be scheduled via **Alteryx Server** or **Windows Task Scheduler** to run daily at 9:00 AM.

## 📈 Business Value
*   **Efficiency:** Reduces reporting time from 30 minutes/day to **0 minutes**.
*   **Real-Time Feedback:** Agents receive immediate feedback on missed targets, allowing for faster course correction.
*   **Scalability:** Can process 10 records or 10,000 records without any change to the workflow structure.

---
*Created by [Your Name]*
