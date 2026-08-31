# Kotak-Finance-Analyzer
This n8n workflow will categorize your financial transactions of any bank and store it in MySQL database
# 🏦 AI-Powered Multi-Bank Transaction Analyzer & Expense Tracker

An automated data pipeline built in **n8n** that parses transaction emails (Kotak Bank, HDFC, ICICI, etc.), extracts metadata, categorizes spending using **Groq LLM**, manages rate limits, and stores structured records in **MySQL** for live Excel analytics.

---

## 🚀 Pipeline Overview

```text
[Gmail Ingestion] ➡️ [JS Regex Parser] ➡️ [Loop Control] ➡️ [Groq LLM Categorization] ➡️ [MySQL Upsert] ➡️ [Excel ODBC Analytics]


Multi-Account Support: Processes transaction alerts across multiple bank accounts via customizable Gmail filters.  JavaScript Metadata Extraction: Extracts core fields including transaction_date, upi_ref_number, recipient_name, recipient_upi_id, amount, and payment_source.  LLM Categorization & Reasoning: Employs the Groq Chat Model and Basic LLM Chain to classify transactions into defined categories with contextual reasoning.  Rate-Limit Control: Uses a Loop Over Items and Wait loop mechanism to process entries smoothly without exceeding LLM rate limits.  Database Upsert: Prevents duplicated records in the upi_transactions table by matching on upi_ref_number.  📋 Prerequisitesn8n Instance (Self-hosted or n8n Cloud)Gmail Account (OAuth2 enabled)Groq API KeyMySQL DatabaseMicrosoft Excel / Power BI (Optional, for ODBC connection)🛠️ Step-by-Step Setup Procedure1. Database SetupRun the following script on your MySQL instance to create the target schema:SQLCREATE DATABASE IF NOT EXISTS expense_tracker;
USE expense_tracker;

CREATE TABLE IF NOT EXISTS upi_transactions (
    transaction_id INT AUTO_INCREMENT PRIMARY KEY,
    transaction_date DATE,
    upi_ref_number VARCHAR(100) UNIQUE,
    recipient_name VARCHAR(255),
    recipient_upi_id VARCHAR(255),
    amount DECIMAL(10, 2),
    payment_source VARCHAR(100),
    category VARCHAR(100),
    ai_reasoning TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
2. Import Workflow into n8nClone or download Kotak Transactions Analyzer.json from this repository.  In n8n, click Workflows ➡️ Import from File, then select the .json file.  3. Add Credentials (Replace API Placeholders)The workflow JSON template uses placeholders (Put your API) for credentials. Connect your own accounts in n8n for:  Gmail OAuth2: Select your credential in Get many messages and Get a message nodes.  Groq API: Add your API key in the Groq Chat Model node.  MySQL: Connect your database in the Insert or update rows in a table node.  4. Configure Bank Accounts & Email TriggersConfigure labels in Gmail for your bank alerts (e.g., Kotak Bank, HDFC, ICICI).In the Get many messages node, modify the filter query parameter (q: label: "Your Bank Label") to target the desired bank account.  5. Execute & AutomateTest the workflow by clicking Execute workflow.  Once verified, toggle the workflow to Active.📊 Live Excel Dashboard Integration (ODBC)Install the MySQL ODBC Connector on your local machine.Add a System DSN connecting to your MySQL expense_tracker database.In Excel, navigate to Data ➡️ Get Data ➡️ From Other Sources ➡️ From ODBC.Load upi_transactions to build live Pivot Tables and automated spending charts[cite: 1].📄 LicenseThis project is licensed under the MIT License.
