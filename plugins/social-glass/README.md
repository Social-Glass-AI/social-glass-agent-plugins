# Social Glass Agent Plugin

Use Social Glass from Codex or Claude Code through the live remote MCP endpoint:

- `https://mcp.socialglass.ai/mcp`

The plugin bundles:

- MCP server config for the canonical Social Glass endpoint
- canonical Social Glass workflow skills from `plugins/social-glass/skills/*`
- Codex and Claude Code marketplace metadata for installation

For setup details, see [Social Glass MCP setup](https://docs.socialglass.ai/reference/mcp-setup).

The bundled skills contain stable, reusable workflows. The hosted MCP server still owns dynamic
runtime guidance: initialization tells clients to call `get_viewer_context`, then `get_instructions`,
and `get_instructions` returns the effective instructions for the authenticated viewer and
organization.

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

The MCP runtime is one hosted endpoint. Codex and Claude Code share one wrapper configuration file:

- `plugins/social-glass/.mcp.json`: shared Codex and Claude Code plugin MCP config

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

## Skills-only install

If the Social Glass MCP is already configured separately, install only the canonical skills through
the skills.sh CLI:

```bash
npx skills add Social-Glass-AI/social-glass-agent-plugins --skill '*' -g -y
```

Update that installation with:

```bash
npx skills update -g -y
```

Use either the skills-only installation or the combined Social Glass plugin so an agent does not
discover duplicate copies of the same skills. The combined plugin remains the default because it
installs the MCP configuration and skills together.

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

For maintainers changing a skill or the plugin wrapper, release from the private app repo:

```bash
pnpm plugins:check-public
pnpm plugins:sync-public
git -C ../social-glass-agent-plugins status --short --branch
git -C ../social-glass-agent-plugins log --oneline origin/main..HEAD
```

Commit and push both this repo and the generated public marketplace repo when skill or wrapper files change.
MCP server/tool behavior changes only need an app deploy unless the plugin metadata or bundled MCP
config changed.

## Auth expectations

- Read access is self-serve for eligible Social Glass users.
- Write tools are role-gated and require a Social Glass admin account.
- The plugin uses the Social Glass-hosted OAuth + MCP flow, not local repo auth.

## First prompts

- `Connect to Social Glass and show my viewer context.`
- `Call list_organizations to find a client organization, then call get_instructions for it.`
- `Search Social Glass zeitlets for one organization.`
- `List projects for an organization and fetch one project with linked contents.`

Use write prompts only after deciding to mutate Social Glass data. Write tools are visible only to
Social Glass admins and must include the intended client `organizationId`; the admin home org is
identity context, not a default client workspace.
