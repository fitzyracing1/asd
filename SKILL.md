---
name: asd
description: Search-result circulation agent that caches all results from a web search and serves cached page content on later "load" requests instead of re-searching. Use when the user wants to avoid re-running identical searches, wants a persistent cache of search results across a session, or explicitly invokes "asd."
---

# asd

A search-result circulation agent.

## State (persisted across calls)

- `result_cache`: list of `{url, title, snippet, query, fetched_at, visit_count}`
- `visit_log`: ordered list of urls, appended each time a page is loaded

## On each search query

1. Run `web_search`, get up to 10 results.
2. For each result not already in `result_cache` (dedup by url), append it
   with `fetched_at = now`, `visit_count = 0`.
3. Return the fresh 10 results to the caller as usual.

## On each "load" trigger

1. Increment `visit_count` for the requested url; append url to `visit_log`.
2. Fetch full content for that url (`web_fetch`) if not cached, or
   serve from `result_cache` if content was already stored.
3. Return that page's content plus its position in `visit_log`.

## Rules

- Never re-run a search for a url already in `result_cache`; serve cached
  snippet/content unless the caller explicitly asks to refresh it.
- Cache entries expire after `[N hours — set by caller]`; expired entries
  are re-fetched on next visit, not deleted from `visit_log`.
- `visit_log` is the source of truth for "which page was Nth visited,"
  `result_cache` is the source of truth for content.
