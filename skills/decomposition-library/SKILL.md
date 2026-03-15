---
name: decomposition-library
description: >
  Activate after every completed decomposition to offer saving.
  Activate when user says "save this", "add to library", "show my library",
  "what have I researched", "past decompositions", "have I done this before",
  or starts a decomposition on a topic that may already exist in the library.
  Always check library/ folder silently at the start of any new decomposition.
---

# Decomposition Library

## Purpose

A personal research library that grows with every session. Every completed decomposition can be saved locally as a structured file. The library is searchable, cross-referenceable, and optionally mirrored to Google Drive or Notion.

---

## Step 0 — Check on New Decomposition Start

At the start of every new decomposition, silently scan the `library/` folder for files whose topic name matches or is close to the current topic.

If a match is found, ask once:

```
I have a past decomposition on [topic] from [date].
Want me to pull it up? It could save us some ground.

1. Yes — load it as context
2. No — start fresh
```

If yes: load the file, surface the key layer conclusions, and use them as the starting point. Skip layers the user already has strong conclusions on unless they want to revisit.

If no: proceed normally. Do not ask again.

---

## Step 1 — Save Prompt After Output

After any output is generated (Value Chain, Mindmap, or Research Note), ask:

```
Save this decomposition to your library? (yes / no)
```

If the user has `library_save` set to `always` in `local.md` — save automatically without asking.

If no — skip. Do not ask again in the same session.

---

## Step 2 — What to Save

Save a structured `.md` file to the `library/` folder in the plugin directory.

**Filename format:** `YYYY-MM-DD-[topic-slug].md`
Example: `2026-03-13-grid-scale-battery-storage.md`

**File contents:**

```markdown
# [TOPIC]
*Decomposed: [Date] | Output: [Value Chain / Mindmap / Research Note]*

---

## Tags
- Sector: [sector]
- Geography: [India / Global / Both]
- Stage: [early / developing / mature]

---

## Layer Summaries

**L0 — Signal**
[2-3 line summary of the signal and why it matters now]

**L1 — Mechanics**
[2-3 line summary of what this actually is]

**L2 — Cause Tree**
[Key root causes identified — bullet list]

**L3 — Solution Space**
[Solution categories and which industries they map to]

**L4 — Build Requirements**
[Key inputs, infrastructure, talent, bottlenecks]

**L5 — Value Chain Actors**
Global: [key companies named]
India: [India proxy companies named with proxy pattern]

**L6 — Research Landscape**
[How the space is accessed, stage of development]

---

## Sources Cited
[List of sources referenced during this session]

---

## Conviction
Score: [blank — to be filled manually or via stress test]
Last reviewed: [date]
Next review trigger: [user-defined or blank]

---

## Notes
[Any freetext notes from the session]
```

---

## Step 3 — Mirror to Drive or Notion (if connected)

After saving locally, check `local.md` for connected sources.

If Google Drive is connected and `library_mirror` is set to `drive`:
```
Saved locally. Mirroring to your Google Drive — 
I'll keep both in sync going forward.
```

If Notion is connected and `library_mirror` is set to `notion`:
```
Saved locally. Adding to your Notion research database.
```

If both connected, use whichever is set as primary in `local.md`.

If neither set, ask once:
```
Want me to also back this up to your connected Drive or Notion?
This keeps your library accessible across devices.

1. Google Drive
2. Notion
3. Local only
```

Save their answer to `local.md` as `library_mirror` — never ask again.

---

## `/curiosity-stack:library` Command

When user runs this command, present:

```
Your Decomposition Library

[X] decompositions saved

Recent:
  → [topic] — [date] — [output type]
  → [topic] — [date] — [output type]
  → [topic] — [date] — [output type]

Options:
1. Search by topic
2. Search by sector or tag
3. Open a specific decomposition
4. Delete a decomposition
5. Export full library to Drive / Notion
```

Search scans filenames and tag fields. Opening a decomposition loads it as context and offers to run a layer delta — comparing it against a fresh decomposition on the same topic to show what has changed.

---

## local.md additions

After setup Section 6, add these fields to `curiosity-stack.local.md`:

```yaml
library_save: always / ask / never
library_mirror: drive / notion / local
library_count: [auto-incremented integer]
```
