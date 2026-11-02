# Self-hosted alternatives

> Sources: AFFiNE Documentation, date unknown; Docmost Pricing, date unknown; Outline Documentation and License, date unknown; Docmost Documentation, date unknown; BookStack Documentation, date unknown
> Raw: [AFFiNE](../../raw/products/affine-self-host.md); [Docmost editions](../../raw/products/docmost-editions.md); [Outline](../../raw/products/outline-community.md); [Backups](../../raw/products/docmost-backups.md); [BookStack](../../raw/products/bookstack.md)
> Updated: 2026-11-02

## Overview

The nearest self-hosted products solve different subsets of the request. Their deployment and licensing models are useful references, but none of the checked editions provides the complete combination of open collaboration, Notion-like databases, fine permissions, and native agent access without a paid boundary.

## Findings

| Product | Useful approach | Boundary for RootNotes |
|---------|-----------------|------------------------|
| AFFiNE | Docker Compose bundles an app, Postgres, Redis, and blob storage for collaborative documents. | Its self-hosted documentation says more workspace seats require a Team Plan or installable license. |
| Docmost Community | Free collaborative pages, spaces, full-text search, and self-hosting. | Its pricing page lists databases, granular page permissions, API keys, and MCP under Business. |
| Outline Community | Document collections, revisions, and portable Markdown/JSON export. | Its documentation says the guest role is absent from the self-hosted community edition; the source is BSL 1.1. |
| BookStack | MIT-licensed wiki with REST API and permissions down to the page. | Its shelf/book/chapter/page hierarchy is a documentation model; Notion-style typed databases and scoped agent knowledge would need separate development. |

Docmost's backup guide identifies a general operational lesson: the database, attachments, and configuration are separate restore inputs. Copying live attachments separately from a database dump can produce an inconsistent snapshot.

## Inference

BookStack is the clearest no-paywall starting point if a conventional wiki is enough. Building a small original core gives RootNotes control over the requested collection model, project scopes, and MCP permissions without fitting them into a fixed document hierarchy. Reusing editor and protocol libraries remains sensible; copying a commercial product's service code is not the proposed route.

## See Also

- [Notion and AppFlowy](notion-and-appflowy.md)
- [Agent interface patterns](agent-interface-patterns.md)
