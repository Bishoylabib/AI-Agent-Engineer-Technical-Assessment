# Automated User Data Pipeline

A fault-tolerant workflow automation built with **n8n** that ingests user data from a public API, processes and filters records, stores them in **Google Sheets**, and sends execution summaries or error notifications via email.

The workflow is designed with retry logic, structured error handling, data transformation, and execution metrics to improve reliability, observability, and operational clarity.

---

## 🔍 Overview

This project demonstrates an automated ETL-style pipeline:

- **Extract** user data from a public API  
- **Transform** and filter records based on business rules  
- **Load** processed data into Google Sheets  
- Provide **execution visibility** through summary and error emails  

The workflow emphasizes resilience and clean data processing practices rather than simple automation.

---

## 🏗 Architecture

Public API (dummyjson.com/users)
↓
HTTP Request Node (with retry logic)
↓
Split Items (normalize array payload)
↓
Apply Top-Level Domain (TLD) filtering to allow only `.com` domains.
↓
Transform Data (Full Name + structured fields)
↓
Append to Google Sheets
↓
Aggregate Metrics
↓
Success / Error Email Notification

---

## ✨ Key Features

- ✅ Retry logic (3 attempts with 1000ms delay)
- ✅ Dedicated error handling path
- ✅ Data transformation & enrichment
- ✅ Aggregated execution metrics
- ✅ Success and failure email notifications
- ✅ Workflow reliability improvements

---

## 📊 Metrics Included in Success Email

- Total users fetched  
- Total users processed  
- Total excluded users  
- Workflow execution time  
- Direct link to the generated spreadsheet  

These metrics provide execution transparency and improve workflow observability.

---

## ⚙️ Technical Implementation

### API Handling

The API returns a single JSON payload containing multiple user objects.  
A **Split Items node** is used to normalize the response into individual records for downstream processing.

### Data Filtering

A Top-Level Domain (TLD) filtering rule is applied to enforce domain-based eligibility.  
Only users with email addresses ending in `.com` are allowed to proceed through the pipeline, while others are excluded from downstream processing.

### Data Transformation

A new `Full Name` field is created by combining: firstName + lastName
Additional structured fields are prepared before insertion into Google Sheets.

### Aggregation Strategy

After inserting rows into Google Sheets, the output becomes multiple records (one per user).  
To prevent multiple success emails from being sent, an **aggregation step** consolidates the data into a single summary object before triggering the notification.

This approach ensures:
- Single notification per execution
- Clean summary metrics
- More professional workflow structure

### Error Handling

If the HTTP request fails:
- The request automatically retries 3 times (1000ms interval).
- If failures persist, execution flows through a dedicated error path.
- An error email is sent containing status and error details.

---

## 🛠 Tech Stack

- **n8n** (workflow orchestration)
- **REST API integration**
- **Google Sheets API**
- **Gmail API**

---

## ▶️ How to Run

1. Import the included JSON workflow into n8n.
2. Authenticate:
   - Google Sheets node
   - Gmail node
3. Click **Execute Workflow**.
4. On success:
   - Verify rows are appended to the spreadsheet.
   - Confirm receipt of the success summary email.
5. To test error handling:
   - Temporarily modify the API URL (e.g., misspell it).
   - Execute the workflow.
   - Confirm that the error email is triggered.

---

## 📸 Screenshots

### Workflow Design
<p align="center">
  <img src="https://github.com/user-attachments/assets/066d8cf6-aae7-4a8d-a6c4-b11fc73d483a" width="750" />
</p>

### Workflow Output
<p align="center">
  <img src="https://github.com/user-attachments/assets/fcbe4004-e928-4bb6-8487-978cf32547e6" width="750" />
  <img src="https://github.com/user-attachments/assets/284ad77f-dee6-4ac2-923a-dd45dfea51ed" width="750" />
</p>

---

## 🧠 Design Decisions

### 1. Explicit Retry Strategy
Instead of relying on a single API call, retry logic was implemented to handle temporary API failures. This improves resilience and reduces false-negative workflow failures.

### 2. Split-Then-Process Pattern
Normalizing the API response into individual items ensures modular downstream processing and simplifies filtering and transformation.

### 3. Aggregation Before Notification
Sending emails directly after Google Sheets insertion resulted in duplicate notifications. Aggregating results before notification ensures:
- Single email per execution
- Accurate summary metrics
- Cleaner control flow

### 4. Dedicated Error Path
Separating the success and error flows improves:
- Maintainability
- Observability
- Debugging clarity

---

## 🚀 Future Improvements

- Add configurable filtering rules (not only `.com` emails)
- Store execution logs in a separate sheet for historical tracking
- Add scheduled triggers instead of manual execution
- Implement Slack/Discord notifications alongside email
- Containerize n8n setup for easier deployment
- Add input validation and schema validation before processing
- Introduce performance benchmarking metrics

---

## 🎯 What This Project Demonstrates

- Workflow orchestration
- API integration
- Data transformation and filtering
- Error handling and fault tolerance
- Aggregation and metric reporting
- Production-style automation design thinking

---

## 📌 License

This project is for educational and portfolio purposes.
