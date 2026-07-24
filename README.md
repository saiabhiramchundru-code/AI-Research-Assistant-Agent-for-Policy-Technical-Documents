#  AI Research Assistant Agent for Policy & Technical Documents

<p align="center">
Build with : 
n8n ,
Google Gemini ,
Qdrant ,
OpenRouter ,
Telegram

</p>

An intelligent **Retrieval-Augmented Generation (RAG)** assistant built using **n8n**, **Google Gemini**, **OpenRouter**, **Qdrant Vector Database**, and **Telegram**. The assistant enables users to upload policy papers, government reports, and technical documents, then interact with them using natural language.

---

## 📑 Table of Contents

- Project Overview
- Problem Statement
- Solution Overview
- System Architecture
- Workflow Screenshot
- Project Workflows
- Data Structure
- n8n Nodes Used
- Workflow Explanation
- Output Examples
- Implementation Steps
- Folder Structure
- Possible Improvements
- Technology Stack
- Real-World Use Cases

---

# Project Overview

Policy papers, government reports, and technical documents are often lengthy and difficult to understand. Finding specific information requires significant manual effort.

This project provides an AI-powered research assistant capable of:

-  Reading uploaded PDF documents
-  Searching information semantically
-  Answering questions using only uploaded documents
-  Summarizing technical and policy reports
-  Listing uploaded documents
-  Maintaining conversation context

The system uses **Retrieval-Augmented Generation (RAG)** to ensure responses are grounded in the uploaded documents instead of relying on general AI knowledge.

---

# Problem Statement

Policy documents, government guidelines, and technical reports are often:

- Long and difficult to read
- Written in complex language
- Poorly structured
- Time-consuming to search manually

Students, researchers, consultants, and professionals spend considerable time identifying relevant sections while important insights can easily be overlooked.

An intelligent assistant is needed to understand these documents and provide accurate, context-aware responses.

---

#  Solution Overview

The AI Research Assistant automates document understanding using a Retrieval-Augmented Generation (RAG) pipeline.

### Workflow

1. User uploads a PDF through Telegram.
2. PDF text is extracted.
3. Text is divided into semantic chunks.
4. Google Gemini generates embeddings.
5. Embeddings are stored in Qdrant Vector Database.
6. User asks questions.
7. AI Agent retrieves relevant document chunks.
8. OpenRouter LLM generates an answer based only on retrieved information.
9. Response is returned through Telegram with source document references.

---

# System Architecture

```text
                 User
                   │
             Telegram Bot
                   │
          Document Upload / Query
                   │
              n8n Workflows
        ┌──────────────────────┼──────────┐
        │                      │          │
 Indexing                  AI Chat     List Documents
 Workflow                  Workflow      Workflow
        │                      │
        ▼                      ▼
Google Gemini Embeddings   OpenRouter Language Model
         |                      │
         ▼                      ▼
Qdrant Vector Database    Qdrant Vector Database 
                               │
                               ▼
                           Telegram Response
```

---

#  Workflow Screenshot


## Document Indexing Workflow

```
assets/document-indexing-workflow.png
```

## AI Chat Workflow

```
assets/ai-chat-workflow.png
```

## List Documents Workflow

```
assets/list-documents-workflow.png
```

---

# ⚙️ Project Workflows

This project consists of **three independent n8n workflows**.

## 1️⃣ Document Indexing Workflow

Responsible for processing uploaded PDF documents.

### Features

- Receives PDF from Telegram
- Downloads uploaded file
- Extracts text from PDF
- Detects blank or corrupted files
- Splits document into chunks
- Generates vector embeddings
- Stores embeddings in Qdrant
- Prevents duplicate uploads
- Sends upload confirmation

---

## 2️⃣ AI Chat Workflow

Acts as the intelligent document assistant.

### Features

- Answers questions from uploaded documents
- Summarizes reports
- Supports document comparison
- Maintains conversation memory
- Lists uploaded documents
- Mentions source documents
- Prevents hallucinations by answering only from retrieved content

---

## 3️ List Uploaded Documents Workflow

Utility workflow used to retrieve all indexed document names.

### Features

- Reads vectors from Qdrant
- Removes duplicate filenames
- Returns uploaded document list

---

#  Data Structure

Each document is converted into multiple chunks before indexing.

```json
{
  "id": "policy.pdf_12",
  "content": "Extracted text...",
  "metadata": {
    "document": "policy.pdf",
    "chunk": 12
  }
}
```

---

#  n8n Nodes Used

## Core Nodes

- Telegram Trigger
- Telegram
- HTTP Request
- Set
- IF
- Code
- Split In Batches
- Execute Workflow
- Stop and Error

### AI Nodes

- AI Agent
- Google Gemini Embeddings
- OpenRouter Chat Model
- Qdrant Vector Store
- Simple Memory
- Default Data Loader
- Tool Workflow

---

# Workflow Explanation

## Document Upload

1. User uploads a PDF.
2. Telegram Trigger receives the file.
3. PDF is downloaded.
4. Text is extracted.
5. Blank/corrupted documents are validated.
6. Duplicate document check is performed.
7. Text is chunked.
8. Embeddings are generated.
9. Chunks are stored in Qdrant.
10. Success message is returned.

---

## Question Answering

1. User asks a question.
2. AI Agent receives the query.
3. Agent selects the appropriate tool.
4. Relevant chunks are retrieved from Qdrant.
5. OpenRouter generates an answer.
6. Source documents are included.
7. Response is sent through Telegram.

---

## Listing Documents

1. User requests uploaded documents.
2. AI Agent invokes List Document Workflow.
3. Workflow retrieves filenames from Qdrant.
4. Duplicate names are removed.
5. List is returned.

---

# 💬 Output Examples

### Upload Success

```
The file Policy2025.pdf was uploaded successfully.
```

---

### Duplicate File

```
The file already exists in the collection.
```

---

### Invalid PDF

```
Upload Failed

The uploaded PDF is blank, corrupted, or unsupported.
```

---

### User Query

```
What is the objective of the National Education Policy?
```

### Response

```
Summary

The National Education Policy aims to improve education quality while promoting flexibility and digital learning.

Key Points

• Improve access to education
• Promote skill development
• Encourage technology adoption

Source Documents

• NEP2020.pdf

Conclusion

The policy focuses on creating an inclusive and future-ready education system.
```

---

# 🚀 Implementation Steps

## Prerequisites

- n8n
- Telegram Bot
- Google Gemini API Key
- OpenRouter API Key
- Qdrant Cloud Account

---

## Installation

### Clone Repository

```bash
git clone https://github.com/yourusername/AI-Research-Assistant-Agent.git

cd AI-Research-Assistant-Agent
```

### Import Workflows

Import the following workflows into n8n.

- document-indexing.json
- ai-chat.json
- list-all-document.json

---

### Configure Credentials

Configure the following credentials inside n8n:

- Telegram API
- Google Gemini API
- OpenRouter API
- Qdrant API

---

### Create Qdrant Collection

Create a collection (for example `newco`) and update the collection name in the workflows if necessary.

---

### Activate Workflows

Enable all workflows.

Start chatting with your Telegram Bot.

---

# 📁 Folder Structure

```text
AI-Research-Assistant-Agent/
│
├── README.md
│
├── assets/
   ├── document-indexing-workflow.png
   ├── ai-chat-workflow.png
   ├── list-documents-workflow.png
   ├── chatbot-demo.png
   ├── document-indexing.json
   ├── ai-chat.json
   └── list-all-document.json


```

---

#  Possible Improvements

- OCR support for scanned PDFs
- Web dashboard
- Multi-user authentication
- Support for DOCX and TXT
- Citation highlighting
- Page-level references
- Hybrid semantic + keyword search
- Voice interface
- Streaming responses
- Dashboard for document management

---

#  Technology Stack

| Technology | Purpose |
|------------|---------|
| n8n | Workflow Automation |
| Google Gemini | Embedding Generation |
| OpenRouter | Large Language Model |
| NVIDIA Nemotron | AI Chat Model |
| Qdrant | Vector Database |
| Telegram Bot API | User Interface |
| JavaScript | Custom Logic |

---

#  Real-World Use Cases

-  Academic Research
-  Government Policy Analysis
-  Legal Document Review
-  Corporate Knowledge Base
-  Technical Documentation Assistant
-  Student Learning Assistant
-  Research Paper Summarization

---


