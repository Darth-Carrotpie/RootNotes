# Notion and AppFlowy

> Sources: Notion Help and Developer Documentation, date unknown; user-owned Notion pages observed in browser, 2026-11-02; AppFlowy Self-Hosted Commercial README, date unknown
> Raw: [Notion databases](../../raw/products/notion-databases.md); [Notion sharing](../../raw/products/notion-sharing.md); [Notion export](../../raw/products/notion-export.md); [Notion history](../../raw/products/notion-history.md); [Notion Skills](../../raw/products/notion-mcp-skills.md); [Browser page structure](../../raw/products/notion-browser-page-structure.md); [AppFlowy self-host](../../raw/products/appflowy-self-host.md)
> Updated: 2026-11-03

## Overview

Notion combines nested pages, database rows that are themselves pages, backlinks, workspace or page sharing, version history, and skills exposed through MCP. AppFlowy shows how a self-hosted collaborative system can package its services, but its current free production distribution limits seats and guests.

## Design lessons

- Treat every database entry as a page so project features, plans, and ideas can contain substantial notes and links.
- Keep a navigable tree for browsing and explicit backlinks for related material. Tree placement and relationships serve different purposes.
- Use permissions at project and subspace boundaries. A share link must not imply access to a whole project.
- Export Markdown and structured data together: Notion's Markdown/CSV export has gaps for block types that lack Markdown equivalents.
- Keep full page history independent of a paid plan. Notion's restore window varies by plan; RootNotes' revision retention should be an owner setting, not a license gate.
- Treat a skill as a permissioned knowledge unit. Notion exposes Skills as pages via MCP and tells clients to fetch the specific skill when a task calls for it. RootNotes can add explicit project and general scopes to that pattern.

## Observed pages in the logged-in browser

The supplied Reverse Engineering Tests page is reached through a Projects breadcrumb. It is a mixed-content page: checklist items, inline links to other project pages, and a bulleted section containing external references. The Turdbots page is linked from it and uses rich-text paragraphs and checklist items. The sidebar presents Recents and Favorites alongside shared and private navigation; these are views of pages, not separate copies.

For RootNotes, this makes mixed blocks and inline page references part of the first useful editor slice. A project workspace needs a landing page and breadcrumb navigation; checklists should remain editable page blocks rather than being forced into a collection. No credential values from either page were retained in the research files.
- AppFlowy's self-hosted free tier allows one user seat and up to three guest editors. Its unlimited workspaces do not remove those person limits, so it does not satisfy RootNotes' collaboration goal.

## Observation limit

The browser view covered the supplied pages' visible and accessibility-exposed structure. It did not establish every hidden child page or the exact stored Notion block JSON.

## See Also

- [Self-hosted alternatives](self-hosted-alternatives.md)
