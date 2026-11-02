# Agent interface patterns

> Sources: MCP TypeScript SDK v1 and v2 documentation, date unknown
> Raw: [MCP v1 server](../../raw/protocol/mcp-typescript-server.md); [MCP v2](../../raw/protocol/mcp-typescript-v2.md)
> Updated: 2026-11-02

## Overview

The MCP TypeScript SDK supports remote agent clients and tools that execute actions. Its v1 documentation describes Streamable HTTP, stateless request handling, and bearer authentication; v2 is now the stable SDK line. RootNotes can put the MCP server beside its API while enforcing the same user and workspace authorization. The implementation must choose a protocol version compatible with its intended clients.

## Design implications

- Expose bounded search, context assembly, and explicit read/write tools rather than one tool that dumps a whole workspace.
- Each MCP request must identify the acting principal. The API evaluates access for that principal, including search results, backlinks, and referenced page titles.
- Use revision preconditions on writes so an agent cannot silently overwrite a human's newer edit.
- A root skill should describe when to request context and which tool to call; project runbooks and skills remain data in RootNotes, selected by scope.

The last three points are RootNotes design choices inferred from the SDK capabilities and the product requirements, not behavior promised by the SDK.

## See Also

- [Self-hosted alternatives](self-hosted-alternatives.md)
