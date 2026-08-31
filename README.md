# Kotak-Finance-Analyzer
This n8n workflow will categorize your financial transactions of any bank and store it in MySQL database
# 🏦 AI-Powered Multi-Bank Transaction Analyzer & Expense Tracker

An automated data pipeline built in **n8n** that parses transaction emails (Kotak Bank, HDFC, ICICI, etc.), extracts metadata, categorizes spending using **Groq LLM**, manages rate limits, and stores structured records in **MySQL** for live Excel analytics.

---

## 🚀 Pipeline Overview

```text
[Gmail Ingestion] ➡️ [JS Regex Parser] ➡️ [Loop Control] ➡️ [Groq LLM Categorization] ➡️ [MySQL Upsert] ➡️ [Excel ODBC Analytics]
