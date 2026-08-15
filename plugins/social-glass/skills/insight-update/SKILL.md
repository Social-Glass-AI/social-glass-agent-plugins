---
name: insight-update
description: Use this skill when asked to update insights.
---

We need to keep our `insights` up to date with recent content (many hundreds of `posts` a week). Your job is to make high-signal updates which we'll review/approve later. We are looking for high-quality work.

- call `get_context({ scope: "global", organizationId: null })` before using any other Social Glass tool
- confirm it reports admin context; stop if it does not, because Social Glass write tools are admin-only
- resolve the intended organization with `list_organizations`; require its `organizationId` before continuing
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
    - **avoid duplicates** by double checking with `search_insights` that it doesn't already exist
        - use `status=["approved", "draft"]` to check **all** relevant `insights`
    - do further research to find supporting `posts` and online context to make sure your `insight` is properly built out
3. **merge/consolidate/dedupe**: identify places we could merge `insights`; execute only after explicit confirmation
    - preserve all important details
    - merge into the highest-quality item

**REMEMBER**

- do not force it, and let us know if there are data quality issues

## Globalizing existing insights

Use this proposal-first loop when the user asks to move organization-specific insight work into the
Global model:

1. Load Global instructions once, then call `list_organizations`. It returns active organizations
   only. Use `get_context({ organizationId, scope: "organization", view: "organization" })` only for plausible targets.
2. Build a deterministic candidate queue before writing. Prioritize approved insights that are:
   - newer;
   - currently linked to an active organization;
   - missing that organization's analysis;
   - likely to contain organization-specific canonical wording; or
   - already placed in an organization dashboard/project.
   Do not infer targeting from authoring provenance alone.
3. Read the exact insight globally, then read it in each plausible organization scope. The
   organization-scoped read confirms current access and returns that pair's current analysis revision.
4. Propose one coherent `update_insight` change:
   - keep global title, description, audience, and content organization-neutral;
   - use `analysisPatch.interpretation` for the organization-specific consequence;
   - use `analysisPatch.opportunity` only for a supported strategic opening;
   - use one to three verb-led `analysisPatch.actionItems` without invented owners or deadlines;
   - use `highlightOps` to explain why existing canonical relationships matter especially to that
     organization. Highlights never create links or change canonical relationship notes.
5. Use the returned `analysisRevision`; use `0` only when the organization has no analysis yet. It is
   an optimistic-concurrency version for that one organization-insight overlay, not a global count.
6. Review the pending proposal. A staged proposal is not a completed migration.

Start with a small reviewed canary. Once the prompt is calibrated, process the deterministic queue in
batches; keep ambiguous, proprietary, or weakly supported pairs in review instead of forcing an
analysis.
