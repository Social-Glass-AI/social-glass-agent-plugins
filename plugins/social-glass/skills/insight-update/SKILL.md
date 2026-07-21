---
name: insight-update
description: Use this skill when asked to update insights.
---

We need to keep our `insights` up to date with recent content (many hundreds of `posts` a week). Your job is to make high-signal updates which we'll review/approve later. We are looking for high-quality work.

- resolve the intended organization with `get_viewer_context` or `list_organizations`; require its `organizationId` before continuing
- grab the most recent 500 to 1000 `posts` for that organization (or use a given date range like the last week)
- grab **all** organization-scoped `insights` with `status=["draft", "approved"]` to understand what already exists
- analyze all of the content and existing insights to see where existing items can be improved or new items must be created

Prepare proposals first. Default changes to drafts or the product's proposal/review flow. Get explicit confirmation immediately before modifying an approved insight, creating a production insight, or merging/deduplicating records.

Following your analysis, do work in the following order

1. **update `insights`**: propose how to bolster and keep existing content up to date
    - add new `posts` to bolster evidence
    - update fields as necessary to evolve the insight
    - add `insight.updates` for meaningful (**client-facing**) changes, these shouldn't be marginal additions, as these are high-signal points shown prominently to clients
    - reformat `insight` if fields do not follow instructions (keep these minimal, targeted, incremental, and **as needed**)
2. **create `insights`** if nothing exists to match your `insight`, after explicit confirmation
    - we have a very high standard for new `insights`, and you should put considerably work into making sure they're supported
    - **avoid duplicates** by double checking with `search_zeitlets` that it doesn't already exist
        - use `status=["approved", "draft"]` to check **all** relevant `insights`
    - do further research to find supporting `posts` and online context to make sure your `insight` is properly built out
3. **merge/consolidate/dedupe**: identify places we could merge `insights`; execute only after explicit confirmation
    - preserve all important details
    - merge into the highest-quality item

**REMEMBER**

- do not force it, and let us know if there are data quality issues
