# Social Glass Agent Plugins

Explore live culture and consumer insights from Social Glass in Codex or Claude Code. The plugin includes the hosted MCP connection and native workflow skills.

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

Enable automatic releases once in `/plugin` → **Marketplaces** → **social-glass** → **Enable auto-update**.

## Skills only

If the Social Glass MCP is already configured separately, install just the shared skills:

```bash
npx skills add Social-Glass-AI/social-glass-agent-plugins --skill '*' -g -y
```

Update with `npx skills update -g -y`. Use either the skills-only installation or the combined plugin so the same skills are not discovered twice.

After installation, start a fresh agent session and complete the OAuth flow when prompted.

For setup details, see [Social Glass agent plugins](https://docs.socialglass.ai/reference/plugins).
