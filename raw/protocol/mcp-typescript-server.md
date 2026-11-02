# Server | MCP TypeScript SDK

> Source: https://ts.sdk.modelcontextprotocol.io/server
> Collected: 2026-11-02
> Published: Unknown

Streamable HTTP can run:

* Stateless – no session tracking, ideal for simple API-style servers.
* Stateful – sessions have IDs, and you can enable resumability and advanced features.

`requireBearerAuth` verifies the `Authorization: Bearer` token of a request with a verifier you supply and puts the resulting `AuthInfo` on `req.auth`.

Tools let MCP clients ask your server to take actions. They are usually the main way that LLMs call into your application.
