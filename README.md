# asd

`asd` is an agent skill definition (`SKILL.md`) for a **search-result circulation agent**. The agent caches every web search result and serves cached content on later "load" requests instead of searching again. There is no code: the skill is meant to be run by an agent that has `web_search` and `web_fetch` tools.

## Behaviour (from SKILL.md)

- State: `result_cache` (url, title, snippet, query, fetched_at, visit_count) and `visit_log` (ordered urls).
- **Search:** up to 10 results, de-duplicated into the cache by url.
- **Load:** increments `visit_count`, appends to `visit_log`, and fetches the page only if it is not cached.
- Rules: never re-search a cached url unless asked to refresh. Expiry is `N` hours, set by the caller. `visit_log` is the source of truth for visit order, and the cache is the source of truth for content.

`docs/index.html` is a static page. The repo has no LICENSE file.

Copyright (c) 2026 Joshua Almeida

## Documentation

- In-repo docs: [docs/](docs/index.md)
- Wiki: [https://github.com/fitzyracing1/asd/wiki](https://github.com/fitzyracing1/asd/wiki)

## Plans

This project is free, and its license stays as is. Support it through [GitHub Sponsors](https://github.com/sponsors/fitzyracing1):

- **Free**: use the code, community support.
- **Pro ($49/month)**: priority issues and support.
- **Team ($199/month)**: priority support for your team plus early access.
