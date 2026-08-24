# 06 · Results

---

## Counted

| | |
| :--- | :--- |
| Workflow nodes | **21** |
| Human approval gates | **1** |
| Infrastructure | **Isolated per client** |

These are counts from the built system: nodes, stages, versions, gates, retries. They are verifiable from the workflow itself.

## What changed in the process

| | Before | After |
| :--- | :--- | :--- |
| **Client data** | One shared pool | Isolated infrastructure per client |
| **Duplicate contact** | Possible and embarrassing | Blocked at the database |
| **Copy review** | Nobody reads it | A person approves in Telegram first |
| **Client visibility** | Ask the agency | A live sheet they can open |
| **Default on failure** | Send anyway | Hold |

## What is deliberately not claimed

No time-saved percentage, cost-reduction figure or throughput multiplier appears in this repository. Those numbers require a measured baseline and a measured after, over a stated period, on a stated definition. Where that measurement exists it will be published with its method. Where it does not, the number is not worth more than the process description above.

> An unsourced percentage in a portfolio is a claim the reader has to take on trust. A node count is a claim they can check.

---

[← 05 · The stack](05-stack.md) · [07 · Limitations →](07-limitations.md)
