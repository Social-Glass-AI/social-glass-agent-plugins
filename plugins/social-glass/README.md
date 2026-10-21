# SocialGlass Agent Plugin

Explore live culture and consumer insights from SocialGlass in Codex or Claude Code.

The plugin includes the hosted SocialGlass MCP connection and native workflow skills. Sign in with your SocialGlass account when prompted.

## Install in Codex

```bash
codex plugin marketplace add Social-Glass-AI/social-glass-agent-plugins
codex plugin add social-glass@social-glass
```

Update with:

```bash
codex plugin marketplace upgrade social-glass
```

## Install in Claude Code

```bash
claude plugin marketplace add Social-Glass-AI/social-glass-agent-plugins
claude plugin install social-glass@social-glass
```

Enable automatic releases once in `/plugin` → **Marketplaces** → **social-glass** → **Enable auto-update**.

For a manual update:

```bash
claude plugin marketplace update social-glass
claude plugin update social-glass@social-glass
```

## Skills only

If the MCP is already configured, install only the workflow skills:

```bash
npx skills add Social-Glass-AI/social-glass-agent-plugins --skill '*' -g -y
```

Update them with `npx skills update -g -y`. Do not install both the combined plugin and the skills-only package on the same agent.

For setup details, see [SocialGlass agent plugins](https://docs.socialglass.ai/reference/plugins).
