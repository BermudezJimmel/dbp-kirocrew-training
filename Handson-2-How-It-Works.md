# Hands-On 2 — Kiro Crew: How It Works

**Module 2 of 3** · [All modules](README.md) · Previous: [Module 1](Handson-1-Account-Token.md) · Next: [Module 3 — Agent Creation](Handson-3-Agent-Creation.md)

---

## Think of it this way

Kiro Crew is **one always-on process** (the Gateway). The dashboard, the CLI, the desktop app, and Slack/Discord are all just windows into that same process. Closing the dashboard tab does not stop anything running inside it.

---

## Step 1: Ask it to read something real

In the dashboard chat, ask it to summarize a real file from your own project — not a toy example. Watch it actually open and read the file.

## Step 2: Set up a scheduled job

Go to the **Schedule** panel (or run the command below) and create a recurring job:
```
kirocrew cron add --cron "0 9 * * 1" "summarize my open DBP tasks"
```
This runs every Monday at 9am.

## Step 3: Run it now, don't wait for Monday

```
kirocrew cron list
kirocrew cron trigger <id>
```
Watch the result come back immediately.

## Step 4: Notice what it does NOT know

Ask it something about a system it has no tool connected for (e.g. "check our production AWS account"). Notice it either gives a generic answer or tells you it has no tool wired in for that. **Capability only exists if the right tool/credential is actually connected — it is not automatic.**

**Good to know:** Crew's security is several layers deep (approval prompts, denied commands, sandboxing, audit logging, and more) — every action is logged, not just trusted silently.

---

Done with Module 2? → [Module 3 — Agent Creation](Handson-3-Agent-Creation.md)
