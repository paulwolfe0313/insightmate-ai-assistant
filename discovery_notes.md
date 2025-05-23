# Discovery Notes – InsightMate AI Assistant

## Client Overview
Acme Consulting Group (fictional) is a mid-sized professional services firm that generates large volumes of internal documentation, including client audit reports, sales reviews, QBRs (Quarterly Business Reviews), and operational summaries. These documents are typically stored in PDF, Word, and Excel formats, scattered across SharePoint and internal folders.

## Identified Pain Point
Executives and department leads spend significant time each week:
- Searching for specific data points or summaries within these reports.
- Compiling summaries manually for status meetings or updates.
- Re-reading the same documents to find patterns or unresolved issues.

This process is inefficient, time-consuming, and error-prone — especially across teams like Sales, HR, and Compliance.

## Project Goal
Build an internal AI assistant that:
- Ingests semi-structured internal reports from PDFs, CSVs, and APIs.
- Allows users to query reports using natural language via Slack or a Web UI.
- Returns accurate, summarized, and traceable responses linked to source documents.
- Logs interactions for transparency and auditability.

## Success Criteria
- A working prototype deployed in a secure environment (or local VPC).
- Answer latency < 3 seconds per query.
- PDF + CSV ingestion covering 80% of report types.
- Integration with Slack for team-wide adoption.
- Executive-friendly dashboard for testing and feedback.

## Constraints
- Sensitive data: no PII/PHI exposure.
- Must run in client’s cloud environment (AWS preferred).
- Slack + web frontend required.
- Must use open-source LLMs unless OpenAI can be justified (cost/performance).

## Ideal User Story
> As a sales executive, I want to ask “How did the Dallas region perform in Q3?” and receive a summarized answer with links to the exact source docs — without opening 10 PDFs.

