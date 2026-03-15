# Changelog

All notable changes to the Curiosity Stack plugin are documented here.

---

## [3.1.0] — 2026-03-13

### Added
- **Decomposition Library** (`skills/decomposition-library/SKILL.md`) — saves every completed decomposition as a structured local file in `library/`. Topic, date, all 6 layer summaries, sources cited, tags, conviction placeholder. Retrieval: plugin checks library at start of new decomposition and asks if user wants to load a past session on the same topic. Optional mirror to Google Drive or Notion.
- **`/curiosity-stack:library`** command — browse, search, open, and delete saved decompositions
- **Watchlist Tracker + Trigger Alerts** (`skills/watchlist-tracker/SKILL.md`) — scheduled monitoring agent. Runs on user-set cadence (daily / weekly / fortnightly). Monitors all watchlist topics for news, regulatory changes, new entrants, and funding. Maps findings to value chain layers. Scores materiality HIGH / MEDIUM / LOW with reasoning. Trigger alerts fire when specific layer conditions are met.
- **Watchlist Agent** (`agents/watchlist-agent.md`) — autonomous agent powering the tracker. Uses web_search and web_fetch. Checks cadence before running. Delivers digest via Gmail (default) and surfaces summary in Cowork.
- **Email digest delivery** — watchlist digest sent directly to user's Gmail on cadence. Subject line prefixed with 🚨 if triggers fired. Includes Cowork deep link.
- **`/curiosity-stack:watchlist`** command — manage watchlist topics, set triggers, configure cadence and email settings
- **Source Credibility** (`skills/source-credibility/SKILL.md`) — personal source registry. Tracks which publications, databases, and analysts are cited at which layer and sector. Post-session rating prompt (max 3 sources, 60 seconds). Plugin-suggested ratings based on usage patterns after 5+ decompositions. Surfaces top-rated sources at L1 and L5 automatically.
- **`library/` folder** — local storage for decomposition files. Includes README.md placeholder.
- **Setup extended** — three new sections (Steps 6-8): decomposition library preferences, watchlist monitoring configuration, source credibility settings.
- **`curiosity-stack.local.md` extended** — new fields for library, watchlist monitoring, and source credibility.

### Changed
- `skills/setup/SKILL.md` — extended from 7 steps to 9 steps with new configuration sections
- `curiosity-stack.local.md` — full template rewrite with all new fields, organised by section
- `plugin.json` — version bumped to 3.1.0, homepage updated to announcement post URL

---

## [3.0.0] — 2026-03-13

### Added
- **Autonomous India Proxy Agent** (`agents/india-proxy-agent.md`) — `/curiosity-stack:india-proxy` now runs a full autonomous research sequence: searches Tracxn, Inc42, Screener.in, and NASSCOM, validates candidates against red flag criteria, and returns a structured shortlist with proxy pattern, access method, validation signals, and watch triggers
- **SEBI Disclaimer Hook** (`hooks/hooks.json`) — automatically appends full SEBI disclaimer to every research output at the `Stop` event; cannot be suppressed
- **`curiosity-stack.local.md`** — personal context file; users fill it once and the plugin reads it on every session; covers context, geography, themes, watchlist, sectors to deprioritise, output preference, and free-text notes
- **Customisation section in README** — three customisation paths documented: local.md editing, skill file editing, and adding custom skills
- **`CHANGELOG.md`** — this file
- **`LICENSE`** — MIT

### Changed
- `curiosity-framework/SKILL.md` — session flow now starts with Step 0: silently read `local.md` and apply user context before intake
- `company-discovery/SKILL.md` — India proxy patterns now framed explicitly as research directions with category examples; no specific company names as illustrative examples
- `sebi-compliance/SKILL.md` — added "Naming Companies — Framing Rules" section with explicit required and forbidden language when surfacing company names at Layer 5
- `commands/india-proxy.md` — updated to route to the autonomous agent
- `plugin.json` — version bumped to 3.0.0
- README — full rewrite including plugin structure diagram, agent description, local.md documentation, and "Making It Yours" section

---

## [2.0.1] — 2026-03-12

### Changed
- Removed all specific company and stock name references from public-facing files (one-pager, README, X thread, Substack post)
- Retained India proxy pattern examples in skill files with neutral category framing
- SEBI compliance language hardened throughout

---

## [2.0.0] — 2026-03-12

### Added
- SEBI compliance skill (`skills/sebi-compliance/`) — always-active, highest priority; covers hard decline triggers, language substitutions, and named company framing rules
- Thesis Stress Test skill (`skills/thesis-stress-test/`) and command (`/curiosity-stack:stress-test`)
- Reading list skill (`skills/reading-list/`) — 5 curated recommendations per decomposition
- Output choice flow — user selects Value Chain / Mindmap / Research Note after decomposition; never auto-generates all three
- MCP connectors expanded to 17

### Changed
- `india-proxy` output choice renamed from "India Proxy" to "india-proxy" for command consistency
- "Bull vs Bear" framing renamed to "Thesis Stress Test" throughout
- Author bio simplified to Substack + X only

---

## [1.0.0] — 2026-03-11

### Initial release
- Six-layer Curiosity Stack framework
- Four commands: setup, decompose, india-proxy, mindmap
- Eight skills: curiosity-framework, setup, company-discovery, output-generator, source-guide, reading-list, sebi-compliance, thesis-stress-test
- MCP connectors: Google Drive, Gmail, Notion
