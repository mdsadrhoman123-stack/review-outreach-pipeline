<img src="assets/banner.svg" alt="Review Outreach Pipeline — Per-client outreach, human-approved" width="100%">

# Review Outreach Pipeline

**Customer records are cleaned, scored and turned into outreach copy — then a human approves in Telegram before anything sends.**

![delivered to client](https://img.shields.io/badge/status-delivered%20to%20client-2F6B52?style=flat-square) ![sector](https://img.shields.io/badge/sector-E--commerce%20/%20reviews-12151B?style=flat-square) ![built with](https://img.shields.io/badge/built%20with-n8n-12151B?style=flat-square) ![Workflow nodes](https://img.shields.io/badge/Workflow%20nodes-21-5B6472?style=flat-square)

| | |
| :--- | :--- |
| **Built for** | White-label / agency delivery |
| **Industry** | E-commerce & local business |
| **Status** | delivered to client |
| **Role** | Designed, built and deployed end to end |

---

### On this page

[The problem](#the-problem) · [What changed](#what-changed) · [How it works](#how-it-works) · [When it breaks](#when-it-breaks) · [The stack](#the-stack) · [Limitations](#honest-limitations) · [Read deeper](#read-deeper)

---

## The problem

Agencies wanted a scalable way to turn customer interactions into public reviews without a generic tool that mixes every client's data together.

The two failure modes are equally bad: a shared system that leaks one client's records into another's, and copy that reads as obviously automated and gets ignored.

So the requirement was per-client isolation and a human approval step — not more volume.

## What changed

| | Before | After |
| :--- | :--- | :--- |
| **Client data** | One shared pool | Isolated infrastructure per client |
| **Duplicate contact** | Possible and embarrassing | Blocked at the database |
| **Copy review** | Nobody reads it | A person approves in Telegram first |
| **Client visibility** | Ask the agency | A live sheet they can open |
| **Default on failure** | Send anyway | Hold |

<sub>Before/after describes the change in process, not benchmarked throughput. Where a number is not measured, it is not claimed.</sub>

## How it works

Records are pulled in, deduplicated in PostgreSQL, scored and written up by an AI step, logged to a sheet for visibility, then held at a Telegram approval gate before export for delivery.

<table>
<tr>
<td width="42" valign="top" align="center"><b>01</b></td><td valign="top"><b>Records come in</b><br>Intake is pulled on a schedule for one client's own instance.</td>
</tr>
<tr>
<td width="42" valign="top" align="center"><b>02</b></td><td valign="top"><b>Duplicates are killed</b><br>Deduplication happens in the database before anything is drafted, so nobody is contacted twice.</td>
</tr>
<tr>
<td width="42" valign="top" align="center"><b>03</b></td><td valign="top"><b>Copy is drafted</b><br>Each interaction is scored and written up rather than dropped into a template.</td>
</tr>
<tr>
<td width="42" valign="top" align="center"><b>04</b></td><td valign="top"><b>The client can see it</b><br>Everything is logged to a sheet, so the client has visibility without access to the workflow.</td>
</tr>
<tr>
<td width="42" valign="top" align="center"><b>05</b></td><td valign="top"><b>A person says yes</b><br>The batch stops at a Telegram approval. If nobody answers, nothing sends.</td>
</tr>
<tr>
<td width="42" valign="top" align="center"><b>06</b></td><td valign="top"><b>Then it delivers</b><br>Only approved records are exported for delivery.</td>
</tr>
</table>

### How it flows

<sub>What happens to the client's work, in the order they experience it. The internal build — node graph, execution order, prompts, thresholds — is deliberately not published.</sub>

```mermaid
flowchart LR
    in(["Customer records come in"])
    prep["Cleaned, scored, written up"]
    ap[/"A person approves it"/]
    go["Approved → delivered"]
    hold["Not approved → nothing sends"]

    in --> prep
    prep --> ap
    ap --> go
    ap -.-> hold

    classDef default fill:#F8F7F3,stroke:#12151B,stroke-width:1px,color:#12151B;
    classDef ok fill:#2F6B52,stroke:#12151B,stroke-width:1px,color:#F5F4EF;
    classDef bad fill:#FEE2E2,stroke:#DC2626,stroke-width:1.5px,color:#7F1D1D;
    class go ok;
    class hold bad;
```

<details>
<summary><b>What the shapes mean</b> — colour is not the only signal</summary>

| Shape | Means |
| :--- | :--- |
| **rounded** | Where the client's process starts |
| **box** | Something the system does |
| **diamond** | A decision point |
| **slanted** | A person has to act |
| **green box** | The good outcome |
| **red box** | Failure path — held, escalated or alerted |

Red appears in exactly one role across every repo in this portfolio: where failure goes. Nowhere else. If you see red, something is being held, escalated or alerted.
</details>

> **Walk it interactively** — [open the demo](https://mdsadrhoman123-stack.github.io/review-outreach-pipeline/) and press **Break it** to watch the failure path light up. Source: [`docs/index.html`](docs/index.html)

## When it breaks

Most automation portfolios show you the happy path. The happy path is the easy half. This is the half that decides whether a system survives contact with a real business.

| What goes wrong | How it is detected | What the system does | Who finds out |
| :--- | :--- | :--- | :--- |
| **Same customer appears twice** | PostgreSQL dedup check | Second record dropped before outreach | Nobody — by design |
| **Copy is wrong or off-tone** | Human reads it at the approval gate | Rejected before send, nothing goes out | The approver decides |
| **Nobody approves** | Approval never returned | Batch stays held — the default is not to send | Pending items visible in the sheet |
| **Intake source returns nothing** | Empty result | Run ends without writing, rather than proceeding on empty data | Alert on an empty run |
| **Export target rejects the batch** | Provider response | Retry, then hold the batch intact | Alert with the batch reference |

The default on an unhandled condition is to **stop and tell someone** — never to continue on a guess. A silent success is the failure mode that costs the most, because nobody goes looking for it.

## The stack

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

### Counted, not estimated

| | |
| :--- | :--- |
| Workflow nodes | **21** |
| Human approval gates | **1** |
| Infrastructure | **Isolated per client** |

<sub>These are counts from the built system — nodes, stages, versions, gates. No efficiency percentages are published here without a stated measurement method.</sub>

## Honest limitations

Every design decision costs something. These are the trade-offs in this build, stated by the person who made them.

- The approval gate is deliberately a bottleneck. Throughput is limited by how fast a human answers, which is the correct trade for outreach in a client's name.
- Per-client isolation means per-client deployment. Better privacy, more instances to maintain.
- Scoring quality depends on how complete the intake record is. Thin records produce thin copy.

## What is not in this repo

- **Client data.** None, in any form. Not anonymised, not sampled.
- **Credentials and endpoints.** Never committed. See [`NOTICE.md`](NOTICE.md).
- **The workflow itself.** No exports, no node graph, no execution order, no prompts, no scoring thresholds, no integration wiring — not sanitised, not partial, not in a screenshot. That is the build, and the build belongs to the engagement that paid for it.

This repository documents *how the problem was thought about* — the failure paths, the trade-offs, the reasoning. That is what tells you whether to hire someone. A copy of the wiring would not.

This is a portfolio repository documenting delivered work. It is not a product you can clone and run against your own accounts.

## Read deeper

| | |
| :--- | :--- |
| [01 · The problem](docs/01-problem.md) | The situation before, in full |
| [02 · The client journey](docs/02-journey.md) | Step by step, from their side |
| [03 · Architecture](docs/03-architecture.md) | Diagrams and the reasoning |
| [04 · Failure handling](docs/04-failure-handling.md) | Every path, and where it lands |
| [05 · The stack](docs/05-stack.md) | What was chosen and what was rejected |
| [06 · Results](docs/06-results.md) | What is measured and what is not |
| [07 · Limitations](docs/07-limitations.md) | The trade-offs, in detail |

---

<img src="assets/cta.svg" alt="If a process depends on someone noticing when it breaks, that is the problem I work on." width="100%">

### Tell me what the process is

I will tell you honestly whether automating it is worth your money — including when the answer is no.

**K MD SAYAD RAHMAN** — AI Automation Engineer  
n8n · AI agents · production reliability  
[LinkedIn](https://www.linkedin.com/in/khandokarsayad) · [More systems](https://github.com/mdsadrhoman123-stack)

