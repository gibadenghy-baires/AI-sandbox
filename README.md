# Vision–SFDC Client-Contact Synching Agent (POC)

## Overview
This repository contains a **Proof of Concept (POC)** workflow designed for **n8n (self-hosted)**.  
The goal is to keep **Client-Contacts** (not Clients, which represent firms in Veritext parlance) synchronized between:

- **Vision** → MS SQL Server (via GraphQL interface, staging = VisionST)  
- **Salesforce** → API via OAuth2  

The workflow is called:  
**`Vision-SFDC Client-Contact Synching Agent`**

---

## Purpose
Ensure that colleagues can understand when they are viewing the same **Client-Contact entity** in either system.  
This POC is **not about new reporting or visualization**. Instead, it focuses on **synchronization and linkage**.

---

## Core Logic
- **Vision → Salesforce**
  - When a new Client-Contact is created in Vision, check Salesforce for an existing record.
  - If found, retrieve Salesforce fields required to complete Vision creation and update the linkage.
  - If not found, create the Client-Contact in Salesforce.
- **Salesforce → Vision**
  - When a new Client-Contact is created in Salesforce, check Vision for an existing record.
  - If found, update Vision with linkage.
  - If not found, create the Client-Contact in Vision.
- **Link Map**
  - Maintain a persistent mapping between **Salesforce IDs** and **Vision IDs**.

---

## Technical Notes
- **Vision Access**: GraphQL endpoint to MS SQL Server (staging database).  
- **Salesforce Access**: API via OAuth2.  
- **Optional Deduplication**: Claude API key available to introduce an AI Agent node for fuzzy matching.  
- **Observability**:
  - Workflow history and logs  
  - Audit trail of sync operations  
  - Error queue with retry/backoff  
  - Alerts (Slack/email placeholders)  
- **Operational Safety**:
  - Runbook + rollback documentation  
  - Error handling nodes in n8n  

---

## Usage

### 1. Clone the repository
```bash
git clone https://github.com/<your-org>/<repo-name>.git
cd <repo-name>
