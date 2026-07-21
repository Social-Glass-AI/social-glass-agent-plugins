---
name: create-signals
description: Use this to create `signals` to provide organization-specific high-value and higher-level analysis.
---

1. resolve the intended organization with `get_viewer_context` or `list_organizations`; require its `organizationId` before continuing
2. grab a wide range of recent `posts` scoped to that organization (this may be 500-1000 recent posts or a specified date range)
3. develop hypotheses for useful `signals`
4. research those hypotheses against organization-scoped internal data
5. use external web search only with sanitized, publicly shareable queries; never include private organization data, internal IDs, or unpublished analysis
6. present the supported signal proposals and get explicit confirmation immediately before creating production records
