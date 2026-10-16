# Inbox Attachment Librarian

An automated Gmail attachment management system built with Activepieces that organizes email attachments into Google Drive and maintains a centralized index in Google Sheets.

The workflow handles both historical emails and incoming attachments, so you can organize existing documents on the first run and keep everything organized automatically afterward.

## What It Does

- **Automatic attachment processing:** Detects new email attachments and processes them without manual intervention.
- **Historical backfill:** Processes attachments from previous emails during the initial run, based on the configured lookback period.
- **Organized file storage:** Stores attachments in Google Drive using a `Sender/Date` folder structure.
- **Centralized logging:** Records attachment metadata and processing details in Google Sheets.
- **Document classification:** Uses AI to identify and categorize relevant documents.
- **Duplicate detection:** Uses file hashing to identify duplicate attachments and reduce redundant uploads.
- **Error handling:** Handles unreadable and password-protected files through a fallback process rather than silently discarding them.

## How It Works

The workflow has two main entry points: historical backfill and continuous monitoring.

### 1. Initial Backfill

When the workflow is initialized, it retrieves emails from the configured historical period and processes their attachments. This allows existing documents to be organized without waiting for new emails to arrive.

### 2. New Attachments

After setup, the Gmail trigger starts the workflow whenever a new attachment arrives. The attachment is processed through the same pipeline used for historical emails.

### 3. Processing Pipeline

Each attachment goes through the following steps:

1. Extract the attachment and relevant email metadata.
2. Filter out files that do not meet the configured criteria.
3. Classify the document where applicable.
4. Check for duplicates using a file hash.
5. Create the required sender and date folders in Google Drive.
6. Upload the attachment to its destination.
7. Record its metadata and processing status in Google Sheets.

### 4. File Organization

Attachments are organized by sender and date to make them easier to locate later.

Example folder structure:

```text
Inbox Attachment Library/
├── sender@example.com/
│   ├── 2026-10-14/
│   │   ├── invoice.pdf
│   │   └── proposal.docx
│   └── 2026-10-15/
│       └── report.xlsx
│
└── another.sender@example.com/
    └── 2026-10-15/
        └── meeting_notes.pdf
```

The folder names and file structure can be adjusted to suit the workflow configuration.

## Google Sheets Integration

Every processed attachment is logged in a Google Sheets spreadsheet, providing a central place to track files without having to browse through Drive manually.

Depending on the configured schema, the index can include:

| Field | Description |
|---|---|
| Original Filename | Name of the received attachment |
| Sender | Email address of the sender |
| Received Date | Date the email was received |
| Document Type | Classified document category |
| Drive Location | Link or path to the stored file |
| File Hash | Hash used for duplicate detection |
| Processing Status | Outcome of the processing workflow |

This makes it easier to search for documents, review processing results, and identify files that need attention.

## Error Handling and Duplicate Detection

### Duplicate Detection

The workflow uses SHA-256 hashing to compare file contents rather than relying exclusively on filenames. This helps identify identical files even when they have different names.

### Unreadable Files

Files that cannot be read or are password-protected can follow a fallback path, allowing the workflow to record the issue and preserve the attachment where supported.

### Unclassified Attachments

If a document cannot be confidently classified, it can be routed to a fallback location rather than being discarded.

## Tech Stack

- **Activepieces:** Workflow automation and orchestration
- **Gmail:** Source of emails and attachments
- **Google Drive:** File storage and folder organization
- **Google Sheets:** Attachment index and processing log
- **AI / LLM:** Document classification
- **SHA-256:** Content-based duplicate detection

## Setup

### Prerequisites

- An Activepieces instance with the required integrations.
- Access to a Gmail account.
- A connected Google Drive account and destination folder.
- A Google Sheets spreadsheet for logging.
- Access to the AI provider used for document classification, if enabled.

### Configuration

1. Import or configure the workflow in Activepieces.
2. Connect the required Google services.
3. Configure the historical backfill period.
4. Set the Google Drive destination and Google Sheets spreadsheet.
5. Review the attachment filters and classification rules.
6. Run the initial backfill and verify the results.
7. Enable the Gmail trigger for ongoing processing.

Make sure the historical backfill and live trigger are configured correctly, including the attachment types you want to support.

## Possible Improvements

- Add retry handling for failed uploads and API requests.
- Improve recovery from partially completed processing runs.
- Expand support for additional file types.
- Add monitoring for failed or skipped attachments.
- Build a dashboard for processing statistics.
- Add periodic checks to reconcile Drive files with the Google Sheets index.

## Why This Project?

Email attachments often end up scattered across inboxes, making it difficult to find documents when they are needed. This project automates the process of collecting, organizing, and indexing those files while keeping a record of what has been processed.

The goal is simple: let Gmail handle incoming documents, Google Drive handle storage, and Google Sheets provide a searchable record of the entire process.


<img width="8192" height="1092" alt="regular flow" src="https://github.com/user-attachments/assets/95d42e39-59d4-48fd-8c90-56ae80c6ba0c" />


---

**Built with Activepieces and Google Workspace.**
