---
name: social-glass
description: Use the Social Glass MCP safely and efficiently. Start with viewer and organization context, default to reads, and use write tools only when the user explicitly asks for mutations and the viewer is an admin.
---

# Social Glass

Use this plugin when you need Social Glass data or edits from Codex or Claude Code.

## Workflow

1. Call `get_viewer_context` first.
2. Call `list_organizations` when org scope is unclear.
3. Default to read-only tools unless the user explicitly asks to create or update content.
4. Use write tools only when `get_viewer_context` shows `isAdmin: true`.
5. For admin users, pass an explicit `organizationId` when working outside the default org.

## Read tools

- `get_viewer_context`
- `list_organizations`
- `list_skills`
- `search_zeitlets`
- `get_zeitlet`
- `list_dashboards`

## Write tools

- `create_insights`
- `update_insights`
- `upsert_personas`
- `update_personas_ops`
- `upsert_signals`
- `upsert_projects`
- `update_projects_ops`
- `upsert_reports`
- `create_dashboard`
- `update_dashboard`

## Guardrails

- Do not assume write access. Write tools are admin-only.
- Social Glass now uses one MCP endpoint: `https://mcp.socialglass.ai/mcp`.
- Write tools are role-gated inside that endpoint and only appear for Social Glass admins.
- Start with reads unless the task clearly requires a mutation.
- If auth fails, re-run the MCP login flow instead of guessing around missing context.
- Keep org scope explicit for admin operations so changes land in the intended workspace.
- Prefer OAuth for normal clients. Use Clerk user API keys for durable local/Codex auth when OAuth refresh or reauth UX is unreliable.
- In Codex, a server configured with `bearer_token_env_var` is in bearer mode. To use OAuth, configure `https://mcp.socialglass.ai/mcp` without `bearer_token_env_var` and run `codex mcp login`.
