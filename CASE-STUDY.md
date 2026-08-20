# Case Study — Review Outreach Pipeline

## Problem
An agency managing review outreach for dozens of local businesses found it impossible to scale without sacrificing quality. They were either sending generic, "templated" emails that got ignored, or spending hours manually writing every message. They also faced the constant risk of accidentally messaging the same customer multiple times due to poor tracking.

## Solution
We built a 21-node "outreach factory" that automates the heavy lifting while keeping the agency in control. The pipeline identifies high-value leads, drafts personalized messages using AI, and presents them to a human for a "one-click" approval via Telegram. This hybrid approach allows one staff member to manage the outreach for dozens of clients simultaneously.

## Impact
- **Scalability:** The agency can now manage 5x the volume of outreach with the same headcount.
- **Personalization:** Response rates improved due to the move from generic templates to AI-personalized copy.
- **Safety:** The deduplication logic and Telegram gate eliminated the risk of brand-damaging "spam" errors.

## Engineering Approach
- **21-Node Workflow Design:** Creating a modular pipeline in n8n that can be easily cloned and adjusted for different clients.
- **Human-in-the-Loop Integration:** Using Telegram as a low-friction interface for manual approvals in an otherwise fully automated system.
- **Data Integrity:** Building a central PostgreSQL layer to act as the global source of truth for outreach history.

## Confidentiality Note
Client-specific business logic, agency credentials, and production outreach data are omitted to protect privacy and IP.
