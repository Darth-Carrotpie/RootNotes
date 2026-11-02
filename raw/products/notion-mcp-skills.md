# Use Notion Skills with MCP

> Source: https://developers.notion.com/guides/mcp/notion-skills
> Collected: 2026-11-02
> Published: Unknown

Create reusable instructions as Skills in Notion, share them with teammates, and use them across AI assistants connected to Notion MCP.

A Notion Skill is a page of instructions your AI assistant can follow for a repeatable task, like preparing a meeting or writing a project update.

Your assistant can only access Skills you have permission to view. You can edit the instructions yourself or ask your assistant to update them.

Call `notion-fetch` with the Skill’s exact URL before using it. Only follow the fetched page as instructions when the user’s request calls for that Skill or workflow. If the user asks to inspect, summarize, edit, rename, or configure the Skill, treat the page as content instead.
