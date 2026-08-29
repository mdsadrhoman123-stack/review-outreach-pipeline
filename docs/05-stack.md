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

## The decisions behind that table

### Why a person approves every send

**What it does.** Nothing leaves in the client's name until someone has said yes, on their phone, at a Telegram gate.

**What was turned down.** Automatic sending with a review of a sample afterwards. Much higher throughput — and by the time you read the sample the wrong message has already arrived in a real customer's inbox.

**What that costs.** Throughput is bounded by how fast a human answers. A deliberate bottleneck, and the correct trade for outreach that carries someone else's name.

### Why the client gets a sheet instead of access to the pipeline

**What it does.** A spreadsheet is written on every run, so the client can see what happened without being handed the automation.

**What was turned down.** Giving the client the workflow itself. Total transparency — and one accidental edit takes the pipeline down, at which point the outage is the client's and the blame is the engineer's.

**What that costs.** The sheet is a copy of state rather than the state itself, so it has to be written to on every run and can lag if a run fails.

### Why deduplication is in a database, not the spreadsheet

**What it does.** Contact history lives in PostgreSQL, so the same person is never approached twice.

**What was turned down.** Checking the sheet before sending. One less component to run — and a spreadsheet has no constraint that can refuse a duplicate row, so the check is advisory rather than enforced.

**What that costs.** Per-client isolation means per-client deployment: better privacy, more instances to maintain. Scoring quality still depends on how complete the intake record is — a thin record produces thin copy.

## The rule that applies to all of them

**Nothing that only one person can operate.** A system that depends on the engineer who built it is a liability for the client, however well it runs on the day it is handed over. Every choice above had to survive that test before the technical merits mattered at all.

---

[← 04 · Failure handling](04-failure-handling.md) · [06 · Results →](06-results.md)
