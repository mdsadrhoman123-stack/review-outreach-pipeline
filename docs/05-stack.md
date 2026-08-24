# 05 · The stack

Each choice, and the reason for it.

---

| Component | Why this one |
| :--- | :--- |
| **n8n** | Orchestration, isolated per client |
| **Outscraper** | Record intake |
| **PostgreSQL** | Deduplication, so nobody is contacted twice |
| **GPT-4o-mini** | Scores the interaction and drafts the outreach copy |
| **Cloudinary** | Hosts the review images |
| **Google Sheets** | Visibility for the client without giving them the workflow |
| **Telegram** | The approval gate — a person, on their phone, before any send |
| **Instantly** | Receives the approved export for delivery |

## What was deliberately not used

- **A hosted automation SaaS.** Client data would transit a third party, and the failure handling would be limited to what that vendor exposes.
- **A bespoke application where automation was enough.** The cheapest system to maintain is the one with the least custom code in it.
- **Anything that could not be redeployed by someone else.** A system only one person can operate is a liability for the client.

---

[← 04 · Failure handling](04-failure-handling.md) · [06 · Results →](06-results.md)
