# Social Glass Agent Plugins

Public marketplace package for the Social Glass Codex and Claude Code plugins, including shared workflow skills.

Both plugins point at the same hosted MCP endpoint:

```text
https://mcp.socialglass.ai/mcp
```

## Codex

```bash
codex plugin marketplace add Social-Glass-AI/social-glass-agent-plugins
codex plugin add social-glass@social-glass
```

## Claude Code

```bash
claude plugin marketplace add Social-Glass-AI/social-glass-agent-plugins
claude plugin install social-glass@social-glass
```

## Skills only

If the Social Glass MCP is already configured separately, install just the shared skills:

```bash
npx skills add Social-Glass-AI/social-glass-agent-plugins --skill '*' -g -y
```

Update those skills later with `npx skills update -g -y`. Use either this skills-only
installation or the combined plugin above so the same skills are not discovered twice.

After installation, start a fresh agent session and complete the OAuth flow when prompted.

For setup details, see [Social Glass MCP setup](https://docs.socialglass.ai/reference/mcp-setup).
