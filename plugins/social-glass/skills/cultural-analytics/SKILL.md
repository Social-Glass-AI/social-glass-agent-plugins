---
name: cultural-analytics
description: Measure or estimate the prevalence, volume, or share of a cultural pattern in Social Glass Posts. Use for questions such as what percentage of Posts mention a topic, product type, behavior, aesthetic, or trend.
---

# Cultural analytics

## When to use this skill

Use this Social Glass skill when a user needs evidence from Social Glass Posts to measure or estimate how common a cultural pattern is. It fits questions about the share of Posts that mention a topic, product type, behavior, aesthetic, or trend. Call `get_context` first and use its returned scope. Ask the user to choose an organization only when the returned context says selection is required.

1. Call `get_context` for the requested organization or Global scope.
2. State the population, time range, denominator, and one simple inclusion rule.
3. Run `search_posts` in `summary` mode for the main term and close synonyms.
4. Review candidates with `outputMode: "select"` and only the fields needed to classify them, usually `shortDescription` and `relevance`.
5. Deduplicate by Post `ref`, apply the inclusion rule, and calculate the result.
6. Report an exact percentage only when the denominator and match count are exact. Otherwise call it a bounded estimate and state the search limits or unknown coverage.

Do not treat a retrieval count below the requested limit as proof of complete coverage.
