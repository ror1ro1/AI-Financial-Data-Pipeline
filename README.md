# AI Financial Data Pipeline

An automated data engineering and analytics pipeline built in Python. The system extracts raw metrics from a relational SQL database, executes analytical aggregations, and orchestrates an AI Financial Agent to generate comprehensive, production-ready markdown business reports.

## Features
- **SQL Aggregation**: Performs native SQL arithmetic inside a relational SQLite database to compute dynamic metrics like net profit margins.
- **AI Contextual Analysis**: Ingests structured JSON data blocks into the Google Gemini 2.5 Flash engine using structured prompting.
- **Automated Document Export**: Automatically compiles and outputs enterprise-grade Executive Briefings in Markdown format.

## Tech Stack
- **Language**: Python 3
- **Database**: SQL / SQLite3
- **AI Core**: Google GenAI SDK (Gemini 2.5 Flash)
