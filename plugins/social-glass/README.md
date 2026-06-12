# Social Glass Agent Plugin

Use Social Glass from Codex or Claude Code through the live remote MCP endpoint:

- `https://mcp.socialglass.ai/mcp`

The plugin bundles:

- MCP server config for the canonical Social Glass endpoint
- Codex and Claude Code marketplace metadata for installation

For setup details, see [Social Glass MCP setup](https://docs.socialglass.ai/reference/mcp-setup).

The plugin does not bundle a usage skill or a copy of the Social Glass agent prompt. The hosted MCP
server owns runtime guidance: initialization tells clients to call `get_viewer_context`, then
`get_instructions`, and `get_instructions` returns the effective Social Glass instructions for the
authenticated viewer and organization.

## Source and generated copies

Edit this package in the private app repo:

```text
plugins/social-glass
```

The public marketplace package is generated into the sibling repo:

```text
../social-glass-agent-plugins/plugins/social-glass
```

Do not edit the Codex marketplace cache directly. It is installer output, commonly under:

```text
~/.codex/.tmp/marketplaces/social-glass
```

## MCP config files

The MCP runtime is one hosted endpoint. The plugin keeps separate wrapper config files because Codex and Claude Code use different JSON shapes for bundled MCP server config:

- `plugins/social-glass/.codex-mcp.json`: Codex plugin MCP config using `mcp_servers`
- `plugins/social-glass/.mcp.json`: Claude Code plugin MCP config using `mcpServers`

Do not fork the server URL or tool contracts between clients.

## Codex team install

For teammates, the best path is a Git-backed marketplace source. They do not need a local clone of this repo just to use the plugin.

```bash
codex plugin marketplace add Social-Glass-AI/social-glass-agent-plugins
codex plugin add social-glass@social-glass
```

After that:

1. Restart Codex if the marketplace does not appear immediately.
2. Open the `social-glass` marketplace in Codex.
3. Install `Social Glass`.
4. Authorize the MCP connection when prompted.

## Claude Code team install

For teammates using Claude Code:

```bash
claude plugin marketplace add Social-Glass-AI/social-glass-agent-plugins
claude plugin install social-glass@social-glass
```

After install, start a new Claude Code session. Use `/mcp` to authenticate if the server asks for OAuth.

If someone only wants to add the MCP server directly in Claude Code without installing the plugin:

```bash
claude mcp add --transport http social-glass https://mcp.socialglass.ai/mcp
```

Then start Claude Code and use `/mcp` to authenticate.

## Claude chat/Desktop connector

For nontechnical teammates who do not use Codex or Claude Code, add the hosted MCP URL as a Claude custom connector:

```text
https://mcp.socialglass.ai/mcp
```

This gives Claude the MCP tools and server initialization instructions from the hosted endpoint. Claude currently supports custom connectors on Free, Pro, Max, Team, and Enterprise plans; Free users are limited to one custom connector.

## Local dev install

If someone already has a local clone and wants to test plugin changes before they land on the shared branch:

```bash
codex plugin marketplace add /absolute/path/to/social-glass
codex plugin add social-glass@social-glass
claude plugin marketplace add /absolute/path/to/social-glass
claude plugin install social-glass@social-glass
```

That marketplace registration is machine-global for that agent client, but it points at the local repo path. If that local path goes away, the marketplace entry will stop working.

## Updating

For a Git-backed marketplace source, pull the latest Codex plugin updates with:

```bash
codex plugin marketplace upgrade social-glass
```

For Claude Code:

```bash
claude plugin marketplace update social-glass
claude plugin update social-glass
```

For a local-path marketplace source, just update the repo checkout. `upgrade` does not apply to local marketplaces.

For maintainers changing the plugin wrapper, release from the private app repo:

```bash
pnpm plugins:check-public
pnpm plugins:sync-public
git -C ../social-glass-agent-plugins status --short --branch
git -C ../social-glass-agent-plugins log --oneline origin/main..HEAD
```

Commit and push both this repo and the generated public marketplace repo when wrapper files change.
MCP server/tool behavior changes only need an app deploy unless the plugin metadata or bundled MCP
config changed.

## Auth expectations

- Read access is self-serve for eligible Social Glass users.
- Write tools are role-gated and require a Social Glass admin account.
- The plugin uses the Social Glass-hosted OAuth + MCP flow, not local repo auth.

## First prompts

- `Connect to Social Glass and show my viewer context.`
- `Call get_instructions for my current Social Glass organization.`
- `Search Social Glass zeitlets for one organization.`
- `List projects for an organization and fetch one project with linked contents.`

Use write prompts only after deciding to mutate Social Glass data. Write tools are visible only to
Social Glass admins and should include the intended `organizationId` when operating outside the
viewer default organization.
