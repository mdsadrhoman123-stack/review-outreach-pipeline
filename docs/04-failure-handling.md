# 04 · Failure handling

The part of the system that took the longest to build and gets written about the least.

---

| What goes wrong | How it is detected | What the system does | Who finds out |
| :--- | :--- | :--- | :--- |
| **Same customer appears twice** | PostgreSQL dedup check | Second record dropped before outreach | Nobody — by design |
| **Copy is wrong or off-tone** | Human reads it at the approval gate | Rejected before send, nothing goes out | The approver decides |
| **Nobody approves** | Approval never returned | Batch stays held — the default is not to send | Pending items visible in the sheet |
| **Intake source returns nothing** | Empty result | Run ends without writing, rather than proceeding on empty data | Alert on an empty run |
| **Export target rejects the batch** | Provider response | Retry, then hold the batch intact | Alert with the batch reference |

## The three rules behind that table

**1 — Fail closed, not open.** When the system cannot establish that an action is safe, it holds. A held item is a visible problem. An item processed on a guess is an invisible one.

**2 — Nothing disappears.** Anything that cannot be completed is recorded where a human can find it later, not dropped from the run.

**3 — Silence is a fault.** An empty result where results were expected is treated as a possible failure of the source, not as an absence of work. This is the check most automations skip.

---

[← 03 · Architecture](03-architecture.md) · [05 · The stack →](05-stack.md)
