# Research Index

This branch records the product comparison and the evidence behind RootNotes. Resume future research here before changing architectural decisions.

## RootNotes product and agent architecture

> Asked: 2026-11-02 — "Investigate and understand Notion and AppFlowy apps ... Run a deeper online research about other similar apps and their approaches. Our goal: Make an app that would function similar how Notion does."
> Status: Active
> Sessions: 2 (last 2026-11-03)

### Questions

| # | Question | Status | Answered in |
|---|----------|--------|-------------|
| Q1 | Which Notion concepts matter for the requested workflows? | Answered | [Notion and AppFlowy](notion-and-appflowy.md) |
| Q2 | Which self-hosted alternatives satisfy unlimited, unpaid collaboration and agent access? | Answered | [Self-hosted alternatives](self-hosted-alternatives.md) |
| Q3 | What architecture supports human and agent workflows with controlled context? | Partial | [Agent interface patterns](agent-interface-patterns.md); [Product proposal](../../docs/product-architecture.md) |

### Articles

| Article | Summary | Updated |
|---------|---------|---------|
| [Notion and AppFlowy](notion-and-appflowy.md) | Reference behavior, observed page structure, and AppFlowy license boundary | 2026-11-03 |
| [Self-hosted alternatives](self-hosted-alternatives.md) | Comparison of AFFiNE, Docmost, Outline, and BookStack | 2026-11-02 |
| [Agent interface patterns](agent-interface-patterns.md) | MCP transport and access boundary | 2026-11-02 |

### Open threads

- The supplied Notion pages were inspected in the logged-in browser. Hidden descendants and stored block JSON remain unverified.
- The supplied [AppFlowy guide](https://appflowy.com/docs/Step-by-step-Self-Hosting-Guide---From-Zero-to-Production) returned a JavaScript shell to text readers. The deployment findings use AppFlowy's official self-host repository and related deployment documentation.
- Confirm the initial editor format and which import and export formats are essential before implementation.
- Validate exact Community edition limitations on a running Docmost instance if a migration path through Docmost is considered.

### Next session

1. Convert the product proposal into a first sprint plan and implement a secure vertical slice.
2. If import fidelity becomes a requirement, inspect or export the hidden descendants and stored block types of the sample pages.
