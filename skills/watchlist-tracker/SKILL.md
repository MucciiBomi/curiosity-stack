---
name: watchlist-tracker
description: >
  Activate when Cowork is opened and watchlist monitoring cadence has elapsed.
  Activate when user says "check my watchlist", "what's new on my watchlist",
  "run watchlist", "any updates", "watchlist digest", or configures monitoring
  via /curiosity-stack:watchlist command.
  Do NOT activate on every session — check last_run date in local.md first.
---

# Watchlist Tracker + Trigger Alerts

## Purpose

A scheduled research monitoring agent. Runs automatically when Cowork is opened and the user's set cadence has elapsed. Monitors every item on the watchlist for material developments, maps findings to value chain layers, scores materiality, and delivers a digest to email (default) and/or surfaces it in Cowork.

---

## Step 0 — Cadence Check

Before doing anything, silently read `local.md`:
- Check `watchlist_last_run` date
- Check `watchlist_cadence` (daily / weekly / fortnightly)
- If cadence has NOT elapsed since last run → do nothing, stay silent
- If cadence HAS elapsed → run the full digest flow below

---

## Step 1 — Research Each Watchlist Item

For each item in the user's watchlist, run parallel research across all signal types the user has prioritised:

**Signal types to monitor:**
- 📰 News — significant coverage, narrative shifts, new analyst coverage
- 🏛️ Regulatory — policy changes, government announcements, compliance shifts
- 🏢 New entrants — companies entering or exiting this value chain layer
- 💰 Funding — rounds raised by companies in this space
- 📊 Earnings / results — for listed companies on the watchlist

**Research sources to check:**
- Web search (recent news, last 7/14/30 days depending on cadence)
- Inc42, Tracxn (for India-specific developments)
- NASSCOM, Ministry announcements (for regulatory)
- Screener.in (for listed India companies)
- User's connected sources (Google Drive, Notion, Gmail) for any saved notes

**Per item, for each signal found:**
- Identify which value chain layer it affects (L0–L6)
- Assess materiality: HIGH / MEDIUM / LOW
- Write a 2-3 line explanation of why it matters (or doesn't)

---

## Step 2 — Trigger Alert Check

After the broad research, check each item's specific triggers from `local.md`:

```yaml
watchlist:
  - name: EV Batteries
    triggers:
      - layer: L4
        condition: "new entrant or funding in annotation/data layer"
      - layer: L6
        condition: "any IPO or pre-IPO development"
```

If a trigger condition is met:
- Flag it separately at the top of the digest as 🚨 TRIGGER FIRED
- Include full detail and reasoning
- These always get HIGH materiality regardless of the agent's assessment

---

## Step 3 — Compile Digest

Structure the digest as follows:

```
CURIOSITY STACK — WATCHLIST DIGEST
[Daily / Weekly / Fortnightly] | [Date]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🚨 TRIGGERS FIRED
[Only if any triggers met — list here with full detail]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

[TOPIC 1] — [HIGH / MEDIUM / LOW]
Layer affected: L[X] — [layer name]
Signal type: [News / Regulatory / New Entrant / Funding]

What happened:
[2-3 lines — factual, specific, no hype]

Why it matters:
[2-3 lines — how this changes or doesn't change the thesis.
Be honest if it's noise. "This is early stage and may not
be material yet — worth watching but not thesis-changing."]

Thesis impact: [Strengthens / Weakens / Neutral / Watch]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

[TOPIC 2] — [HIGH / MEDIUM / LOW]
[same structure]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

[TOPIC with no material change]
No material developments this period. ⚪

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Open Cowork to go deeper on any of these →
[cowork deep link]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Curiosity Stack · Adjust digest settings:
/curiosity-stack:watchlist
```

---

## Step 4 — Deliver Digest

**Email delivery (default):**

Use Gmail MCP connector to send the digest to the user's confirmed email address (stored in `local.md` as `watchlist_email`).

Email format:
- Subject: `Curiosity Stack — [Cadence] Digest | [Date] | [X] updates`
- Body: full digest as above
- If any TRIGGER FIRED: subject line prepends 🚨

**Cowork surface (always):**

Regardless of email setting, surface a summary at the top of the session:

```
Watchlist digest ready — [X] updates, [Y] triggers fired.
[View digest] or [Skip for now]
```

If user clicks View: show full digest inline.
If user clicks Skip: archive it, accessible via `/curiosity-stack:watchlist`.

---

## Step 5 — Update local.md

After running, update:
```yaml
watchlist_last_run: [today's date]
```

---

## `/curiosity-stack:watchlist` Command

When user runs this command:

```
Your Watchlist

Monitoring: [cadence] | Last run: [date] | Next run: [date]
Email digest: [on/off] → [email address]

Tracked topics:
  → [topic 1] — [X triggers set] — last: [HIGH/MEDIUM/LOW/none]
  → [topic 2] — [X triggers set] — last: [HIGH/MEDIUM/LOW/none]

Options:
1. Add topic to watchlist
2. Remove topic
3. Add / edit trigger for a topic
4. Change cadence
5. Toggle email digest
6. View last digest
7. View digest archive
```

**Adding a trigger:**
```
What topic are you setting a trigger for?
→ [user names topic]

Which layer should I watch?
L0 Signal / L1 Mechanics / L2 Causes / L3 Solutions / 
L4 Requirements / L5 Actors / L6 Research Landscape

What specific condition?
Describe in plain language — e.g. "new company enters this layer"
or "regulatory change" or "funding round above $10m"
```

Save trigger to `local.md` under the relevant watchlist item.

---

## local.md additions

```yaml
watchlist_cadence: daily / weekly / fortnightly
watchlist_last_run: [date]
watchlist_email: [email address — auto-detected from Gmail connector]
watchlist_email_enabled: true / false
watchlist_cowork_summary: true / false

watchlist:
  - name: [topic]
    added: [date]
    last_status: HIGH / MEDIUM / LOW / none
    signal_priorities:
      - news
      - regulatory
      - new_entrants
      - funding
    triggers:
      - layer: L[X]
        condition: "[plain language condition]"
        last_fired: [date or never]
```
