# RootNotes product and architecture proposal

Status: **proposed**, 2026-11-02. This is a build target, not an implemented feature list. The [research index](../wiki/research/index.md) contains the evidence and open questions.

## Product promise

One self-hosted instance holds personal notes, project workspaces, and reusable knowledge. Humans use a Notion-like web interface. Agents connect through MCP, ask for the smallest relevant context, and create or revise durable project material. RootNotes imposes no license-based limits on users, guests, workspaces, pages, or skills; available storage and server capacity still bound an installation.

## User model

| Concept | Meaning | Typical content |
|---------|---------|-----------------|
| Personal workspace | Private area owned by one user | Meeting notes, networking notes, research, ideas |
| Project workspace | One workspace bound to a repository or project | Plan, runbook, wiki, decisions, work log, features |
| General workspace | Shared knowledge that can apply to many projects | Work rules, reusable skillbook, templates |
| Subspace | Access boundary within a workspace | Friends, client, team, or restricted project area |
| Page | Addressable, versioned content with nested child pages | Note, plan, log, wiki article, feature, skill |
| Collection | Typed set of pages with views | Ideas database, feature tracker, research sources |
| Link | Typed relation between pages | Mentions, supports, depends on, supersedes, related to |

The project workspace is the default boundary for a repository. A subspace is a child access boundary, not a new repository. The general workspace is separate so a rule can apply across repositories without copying its text into each one. A project landing page provides the breadcrumb and mixed-content entry point observed in the supplied Notion pages; Recents and Favorites are navigation views over existing pages.

## Essential human workflows

1. Create a personal note or project page with a title, paragraphs, checklists, bullet lists, inline page links, external links, and attachments. Nest pages in a sidebar and find them with search and backlinks.
2. Create an idea or feature collection. Add a row as a full page, define typed properties, and use table and board views. Later views can add calendar and gallery layouts.
3. Create a plan, discuss it, and keep its changes in page history. Restore produces a new revision, preserving the previous state.
4. Invite a friend or colleague to a project or subspace as owner, editor, commenter, or viewer. Removal revokes their content and MCP access promptly.
5. Export a page or workspace as Markdown plus attachments and a machine-readable manifest. Add HTML and PDF render exports after the core data model stabilizes. Import must report unsupported blocks instead of dropping them silently.
6. Schedule database, attachment, and configuration backups to a second location. A restore drill is part of release acceptance.

## Essential agent workflows

1. A small RootNotes root skill tells agents how to connect, identify a repository, query context, and save useful outcomes. It contains tool guidance, not all knowledge.
2. When asked to build a branch or research a topic, the agent resolves the repository identity, finds or creates its project workspace, and reads the relevant runbook, plan, and related pages.
3. Agent-created artifacts use explicit types: plan, research note, decision, runbook, wiki article, work log, and skill. Each carries author, source links, scope, revision, and review state.
4. After work, the agent updates the plan and work log and links changed decisions or source notes. It does not replace a newer human edit.
5. General work rules and transferable skills live in the general workspace. Skills declare `global`, `project`, or `subspace` scope, applicability conditions, owner, version, status, and last review date. A project-specific skill can override general guidance where both apply, subject to the user's current instruction.
6. An agent can propose a new reusable skill or a revised version with source links and a diff. Broadly scoped rules and skills become active after an owner reviews them; project logs and ordinary notes can be written directly within the token's permissions. Review dates and source changes create maintenance reminders.

## Data and access rules

- Store stable UUIDs for every workspace, subspace, page, collection, row, attachment, revision, and relation. Human-readable slugs can change without breaking links.
- Page content is structured editor JSON, with an indexed plain-text projection and a Markdown export. Keep unknown imported blocks intact where possible; warn on conversion losses. A Markdown authoring path may be offered for agents and technical users.
- A collection row is a page with typed property values. Table and board are views of the same rows, not separate copies.
- Relations are explicit rows with source, target, type, and optional label. Generate backlinks from permitted relations. Broken links are visible in a maintenance view; deleted page IDs are never silently reused.
- Permissions inherit from workspace to subspace to page. Private subspaces can restrict inherited access. Every request checks the effective role against the target; search, link previews, backlinks, exports, and MCP use the same check. Do not reveal inaccessible titles or snippets through relations.
- Registration is invite-only by default. Invitations expire and are bound to an email or existing account. No seat counter controls invitations.
- A principal is a user or a scoped agent token acting for a user. Tokens carry allowed workspaces, operations, expiry, and revocation state. Store only token hashes. Log agent writes with both the token identity and acting user.
- Every write includes an expected revision. A conflict returns the latest revision and a diff or comparison path. Restore and edit create new revisions. Store an immutable audit event for permission changes and agent writes.

### Proposed records

| Record | Minimum fields |
|--------|----------------|
| `users` | ID, login identity, password hash or identity provider subject, disabled state |
| `workspaces` | ID, kind, owner, title, repository identity when relevant |
| `spaces` | ID, workspace ID, parent space ID, title, inherited or restricted access mode |
| `memberships` | Principal, workspace or space, role, invitation state |
| `pages` | ID, workspace/space/parent IDs, type, title, editor JSON, text projection, current revision, review state, source references, deleted state |
| `page_revisions` | Page ID, number, content snapshot, author, timestamp, restore source |
| `collections` / `properties` / `values` / `views` | Schema, typed row values, saved filters and layouts |
| `links` | Source page, target page, relation type, validity state |
| `attachments` | ID, page, storage key, media type, size, checksum |
| `applicability` | Page ID, scope, include/exclude project tags, conditions |
| `skills` | Page ID, version, active or proposed status, last review |
| `audit_events` | Actor, action, target, timestamp, safe metadata |

Repository identity should use the normalized Git remote host and repository path, with an explicit alias for forks, renamed remotes, or local-only repositories. A branch belongs to a project workspace and can have a linked plan or work log; branch names do not create a new workspace automatically.

## Context assembly contract

The root skill should call `resolve_project`, then `get_context(task, project_id, budget, intent)`. The API first filters by authorization and scope, then retrieves exact project material and applicable general rules, and finally expands only directly useful links. Results return concise excerpts with page IDs, links, revision IDs, source and review status. A caller can follow a link or request a fuller page deliberately.

Selection order: current user instruction; explicit task or page references; repository runbook and active plan; applicable project skills; applicable general rules and skills; supporting research. A local skill may narrow general advice, but content in a page is never allowed to override the agent's higher-priority instructions. Conflicting or stale knowledge is shown as a conflict, not silently merged. Search and context assembly must not traverse into a workspace the principal cannot read.

Initial MCP tool surface:

| Tool | Purpose |
|------|---------|
| `whoami` | Show token identity and allowed workspaces |
| `resolve_project` | Find or create a workspace for a repository, idempotently |
| `search` | Search authorized titles, text, types, and properties |
| `get_context` | Return a bounded task-specific packet with citations |
| `get_page` / `list_children` / `list_links` | Deliberate navigation |
| `create_page` / `update_page` | Version-checked content writes |
| `link_pages` | Add a typed relation within access rules |
| `append_log` | Append a dated operation entry without replacing earlier entries |

The MCP service validates a bearer credential and calls the same application service layer as the web API. Tool names and results stay stable and versioned. Root skill installation should be possible per agent client, while skillbook content remains on the server.

## Deployment and implementation direction

Use a TypeScript monorepo: a React web app with an open-source Tiptap editor, an HTTP API, and a separate MCP service. PostgreSQL holds records, revisions, and indexed text. Uploaded files live in a persistent volume initially, with an S3-compatible storage adapter only when needed. The web container serves static assets and proxies `/api` and `/mcp`; PostgreSQL and the upload volume are private to the Compose network. Use TLS at the public edge. The [Tiptap documentation](https://tiptap.dev/docs/editor/getting-started/overview) describes its MIT-licensed open-source editor core; the [PostgreSQL documentation](https://www.postgresql.org/docs/current/textsearch.html) supports full-text indexing without a separate search service. Select and pin an [MCP SDK v2](https://ts.sdk.modelcontextprotocol.io/v2/) version compatible with target agent clients when implementation starts.

Compose services: `web`, `api`, `mcp`, `postgres`, plus a scheduled backup job. Redis, a vector database, a second search engine, a mail server, and realtime collaboration infrastructure are additions only when a measured need or accepted user flow requires them. Simultaneous edits in the first slice use revision conflict detection; realtime shared cursors can follow with a CRDT service.

## Delivery slices and acceptance

| Slice | User-visible result | Acceptance gate |
|-------|---------------------|-----------------|
| 1. Secure core | Local accounts, personal and project workspaces, subspaces, roles, mixed-block pages, inline links, search, history, Docker Compose | Two users can edit a shared subspace; a third cannot find or read it; checklist and page links survive edit/export; stale writes fail without data loss; restart preserves data |
| 2. Agent core | MCP service, scoped tokens, root skill, repo resolution, context packets, agent-authored pages and logs | Agent creates a repo workspace, retrieves only applicable knowledge, writes with attribution; revoked token loses access; link traversal cannot expose a private page |
| 3. Structured work | Collections, typed properties, table and board views, ideas and features, skillbook conditions | Row pages and views stay consistent; skill selection respects scope; export preserves all typed values |
| 4. Portability and operations | Markdown/HTML/JSON export, import report, attachments, scheduled off-host backups, restore path | Export can be re-imported without silent loss; a fresh instance restores content, users, permissions, and attachments from a scheduled backup |
| 5. Collaboration polish | Comments, richer blocks, realtime multi-editor sync, PDF export | Concurrent editing and restore keep history consistent across clients |

No slice is complete solely because its containers are healthy. The acceptance gates include authorization, recovery, and round-trip checks.

## Decisions still open

- Choose the repository's distribution license before publishing the application. The product design has no seat enforcement or paid feature gates.
- The supplied Notion pages were inspected in the logged-in browser; a full export would be needed to verify hidden descendants and exact block JSON for a high-fidelity importer.
- Confirm which file formats are essential on day one. The proposed first slice makes Markdown and a lossless manifest the portability baseline.
- Confirm whether guests must be able to edit individual pages outside a shared subspace. The proposed first slice uses subspace sharing, which matches the stated access need with a simpler permission model.
