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

A scheduled research monitoring agent. Runs automatically when Cowork is opened and the user's set cadence has elapsed. Monitors every item on the watchlist for material developments, maps findings to value chain layers, scores materiality, and delivers a digest to email (default) and surfaces it in Cowork.

---

## IMPORTANT — local.md Write Constraint

This plugin cannot write directly to local.md during a session due to Cowork's file system sandbox. Instead, after any configuration change, generate the exact YAML block the user needs to paste and instruct them clearly:

```
Your watchlist has been configured. To save this permanently,
open curiosity-stack.local.md and paste the following under
watchlist_topics:

[exact YAML block]

Open the file with:
  Windows: notepad "C:\path\curiosity-stack.local.md"
  Mac: open -e ~/path/curiosity-stack.local.md

Save and close. I'll read it on your next session.
```

Always generate the complete YAML block — never partial. Never ask the user to write YAML manually.

---

## Step 0 — Cadence Check

Before doing anything, silently read local.md:
- Check `watchlist_last_run` date
- Check `watchlist_cadence` (daily / weekly / fortnightly)
- If cadence has NOT elapsed → do nothing, stay silent
- If cadence HAS elapsed → run the full digest flow below

---

## Step 1 — Research Each Watchlist Item

For each item in the user's watchlist, run parallel research across all signal types the user has prioritised:

**Signal types:**
- 📰 News — significant coverage, narrative shifts
- 🏛️ Regulatory — policy changes, government announcements
- 🏢 New entrants — companies entering or exiting this value chain layer
- 💰 Funding — rounds raised by companies in this space
- 📊 Earnings / results — for listed companies

**Research sources:**
- Web search (recent news, last 7/14/30 days depending on cadence)
- Inc42, Tracxn (India-specific)
- NASSCOM, Ministry announcements (regulatory)
- Screener.in (listed India companies)
- User's connected sources

**Per item, for each signal found:**
- Identify which value chain layer it affects (L0–L6)
- Assess materiality: HIGH / MEDIUM / LOW
- Write 2-3 line explanation of why it matters

---

## Step 2 — Trigger Alert Check

After broad research, check each item's specific triggers:

If a trigger condition is met:
- Flag as 🚨 TRIGGER FIRED at top of digest
- Include full detail and reasoning
- Always HIGH materiality regardless of agent's assessment

---

## Step 3 — Compile Digest

```
CURIOSITY STACK — WATCHLIST DIGEST
[Daily / Weekly / Fortnightly] | [Date]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🚨 TRIGGERS FIRED
[Only if any triggers met]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

[TOPIC 1] — [HIGH / MEDIUM / LOW]
Layer affected: L[X] — [layer name]
Signal type: [News / Regulatory / New Entrant / Funding]

What happened:
[2-3 lines — factual, specific]

Why it matters:
[2-3 lines — how this changes or doesn't change the thesis]

Thesis impact: [Strengthens / Weakens / Neutral / Watch]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

[TOPIC with no material change]
No material developments this period. ⚪

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Open Cowork to go deeper → claude.ai/cowork
Plugin: Curiosity Stack by Ameya Pimpalgaonkar

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
⬡ Curiosity Stack · #CuriosityStack
finstor85.substack.com
Not investment advice. Research purposes only.
```

---

## Step 4 — Deliver Digest

**Email delivery (default):**
Use Gmail MCP connector.
Subject: `Curiosity Stack — [Cadence] Digest | [Date] | [X] updates`
If triggers fired: prepend 🚨 to subject.

**Cowork surface (always):**
```
Watchlist digest ready — [X] updates, [Y] triggers fired.
[View digest] or [Skip for now]
```

---

## Step 5 — Generate YAML for local.md update

After running, generate the update block:

```
Update your curiosity-stack.local.md — paste this:

watchlist_last_run: [today's date]
```

---

## `/curiosity-stack:watchlist` Command

```
Your Watchlist

Monitoring: [cadence] | Last run: [date] | Next run: [date]
Email digest: [on/off] → [email]

Tracked topics:
  → [topic 1] — [X triggers set] — last: [HIGH/MEDIUM/LOW/none]
  → [topic 2] — [X triggers set] — last: [HIGH/MEDIUM/LOW/none]

Options:
1. Add topic
2. Remove topic
3. Add / edit trigger
4. Change cadence
5. Toggle email digest
6. View last digest
7. View digest archive
```

After any configuration change, generate the complete YAML block for the user to paste into local.md.

---

## local.md additions

```yaml
watchlist_cadence: daily / weekly / fortnightly
watchlist_last_run: [date]
watchlist_email: [email]
watchlist_email_enabled: true / false
watchlist_cowork_summary: true / false
session_count: 0

watchlist_topics:
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
