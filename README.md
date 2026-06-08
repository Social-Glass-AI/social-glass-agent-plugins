# Social Glass Agent Plugins

Public marketplace package for the Social Glass Codex and Claude Code plugins.

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

After installation, start a fresh agent session and complete the OAuth flow when prompted.

For setup details, see [Social Glass MCP setup](https://docs.socialglass.ai/reference/mcp-setup).
