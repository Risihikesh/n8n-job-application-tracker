# n8n Job Application Tracker

An automated job application tracking workflow built with **n8n, Gmail, Gemini AI, and Google Sheets**.

The workflow monitors incoming emails, identifies job-application-related emails, extracts structured application information using AI, and automatically maintains a job application tracker in Google Sheets.

## 🚀 Workflow Overview

```text
Gmail
  ↓
Get Emails from Last 12 Hours
  ↓
Rule-Based Email Filtering
  ↓
Gemini AI Classification & Extraction
  ↓
Check Existing Applications
  ↓
    ┌───────────────┐
    │ Existing Job? │
    └───────┬───────┘
        Yes │ No
            │
     ┌──────┴──────┐
     ↓             ↓
Update Row     Append Row
     ↓             ↓
     └──────┬──────┘
            ↓
      Google Sheets
```

## ✨ Features

* Automatically processes job-related emails from Gmail.
* Searches emails from the previous **12 hours**.
* Uses rule-based filtering before sending emails to Gemini.
* Reduces unnecessary AI API requests by filtering obvious non-job emails.
* Detects application confirmations and recruitment-related emails.
* Uses Gemini to classify and extract job application information.
* Extracts structured fields such as:

  * Company
  * Position
  * Source
  * Applied Date
  * Status
  * Job URL
  * Email Subject
  * Notes
* Checks Google Sheets for an existing application.
* Updates an existing row when a matching application is found.
* Appends a new row when the application does not already exist.
* Supports application emails from different sources such as company websites, LinkedIn, Naukri, and Indeed.
* Runs using Docker for local development.

## 🧠 Email Filtering

The workflow uses two stages of filtering.

### 1. Rule-Based Filtering

The workflow checks email subject, snippet, and sender for recruitment-related signals.

Examples include:

```text
application
applied
candidate
interview
assessment
recruiter
hiring
shortlisted
next steps
job offer
```

It also identifies common job-alert patterns such as:

```text
job alert
job recommendations
new jobs
job matches
apply now
saved jobs
job digest
```

Obvious non-job emails such as order confirmations, payments, invoices, OTPs, and promotional emails can be filtered before reaching the AI step.

### 2. Gemini Classification

Emails that appear potentially recruitment-related are passed to Gemini.

Gemini determines whether the email represents a relevant job/application event and extracts the required structured information.

This approach helps avoid sending every Gmail message to the AI API.

## 📊 Google Sheets Structure

The extracted information is stored in Google Sheets.

Example:

| Email Subject          | Company              | Position          | Source          | Applied Date | Status  | Job URL      | Notes                    |
| ---------------------- | -------------------- | ----------------- | --------------- | ------------ | ------- | ------------ | ------------------------ |
| Thank You for Applying | Example Technologies | React Developer   | Company Website | 2026-09-29   | Applied |              | Application received     |
| Application sent       | Example Company      | Frontend Engineer | LinkedIn        | 2026-09-29   | Applied | LinkedIn URL | Application confirmation |

## 🔄 Duplicate Handling

Before adding an application, the workflow checks existing Google Sheets rows.

```text
New application
      ↓
Get existing rows
      ↓
Find matching application
      ↓
     IF
   /    \
Yes      No
 |        |
Update   Append
Row       Row
```

This prevents the same application from unnecessarily creating multiple rows.

## ⏰ Email Processing Window

The Gmail search is configured to process emails from the previous **12 hours**.

This allows the workflow to work together with the 12-hour automation schedule without continuously processing the entire mailbox.

## 🛠️ Tech Stack

* **n8n** — Workflow automation
* **Gmail** — Email source
* **Gemini API** — Email classification and information extraction
* **Google Sheets** — Application database/tracker
* **Docker** — Local n8n environment
* **JavaScript** — Custom n8n expressions and filtering logic

## 📁 Project Structure

```text
n8n-job-application-tracker/
│
├── workflow/
│   └── Job Application Tracker.json
│
├── .env.example
├── .gitignore
├── compose.yml
├── searxng-settings.yml
└── README.md
```

## 🐳 Running Locally

### Prerequisites

* Docker
* Docker Compose
* Gmail account
* Google Sheets
* Gemini API key

### 1. Clone the repository

```bash
git clone https://github.com/Risihikesh/n8n-job-application-tracker.git

cd n8n-job-application-tracker
```

### 2. Create your environment file

```bash
cp .env.example .env
```

On Windows PowerShell:

```powershell
Copy-Item .env.example .env
```

Update `.env` with your own configuration.

**Never commit `.env` to GitHub.**

### 3. Start n8n

```bash
docker compose up -d
```

Check the containers:

```bash
docker compose ps
```

### 4. Open n8n

Open your local n8n instance in the browser and import the workflow from:

```text
workflow/Job Application Tracker.json
```

## 🔐 Credentials

The workflow requires credentials for:

* Gmail
* Google Sheets
* Gemini/API provider

Credentials should be configured inside n8n.

**Do not store API keys, OAuth tokens, passwords, or `.env` files in GitHub.**

The repository contains `.env.example` only as a configuration reference.

## 🔧 Customization

You can customize:

* Gmail search query
* Email processing interval
* Recruitment keywords
* Non-job email filters
* Gemini classification prompt
* Google Sheets columns
* Duplicate matching logic
* Application status values
* Supported job platforms

## 📌 Example Use Case

Instead of manually checking Gmail every day:

```text
Job application email
        ↓
Gmail
        ↓
n8n
        ↓
Filter irrelevant emails
        ↓
Gemini extracts application details
        ↓
Check Google Sheets
        ↓
Update existing application
       OR
Add new application
```

The result is a centralized job application tracker that requires minimal manual maintenance.

## 🔒 Security

This repository is intended to contain the **workflow configuration**, not private credentials.

Never commit:

```text
.env
API keys
OAuth tokens
Passwords
Google service-account credentials
Private authentication data
```

Use environment variables and n8n credentials for sensitive configuration.

## 🚧 Future Improvements

Potential improvements include:

* Better duplicate detection
* More robust email classification
* Support for additional job platforms
* Application status tracking
* Interview-date extraction
* Automatic reminders
* Dashboard and analytics
* Better batch processing
* Further reduction of unnecessary AI API requests
* Cloud deployment

## 👨‍💻 Author

**Rishikesh**

Frontend / MERN Stack Developer

This project was built as a practical automation project to streamline the job application tracking process using workflow automation and AI.
