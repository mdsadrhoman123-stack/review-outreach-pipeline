# 01 · The problem

**Review Outreach Pipeline** — White-label / agency delivery

---

Agencies wanted a scalable way to turn customer interactions into public reviews without a generic tool that mixes every client's data together.

The two failure modes are equally bad: a shared system that leaks one client's records into another's, and copy that reads as obviously automated and gets ignored.

So the requirement was per-client isolation and a human approval step — not more volume.

## Why it was not solved already

Every business in this position has already tried the obvious answers: a shared inbox, a spreadsheet, a rule in an off-the-shelf tool, a reminder to be more careful. Those work until volume grows or someone is on holiday.

The gap is not effort. It is that the process lives in people's habits rather than in a system, so it degrades quietly and nobody can measure by how much.

## What the requirement actually was

Records are pulled in, deduplicated in PostgreSQL, scored and written up by an AI step, logged to a sheet for visibility, then held at a Telegram approval gate before export for delivery.

---

[← README](../README.md) · [02 · The client journey →](02-journey.md)
