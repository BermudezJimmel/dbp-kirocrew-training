# Hands-On 3 — Agent Creation

**Module 3 of 3** · [All modules](README.md) · Previous: [Module 2](Handson-2-How-It-Works.md)

---

## Think of it this way

A custom agent (a "crew member" / "mate") is like **onboarding a new associate-level hire**. You don't hand a new hire the keys to everything day one. You give them:
- A **job description** — what they're there to do
- A **toolbox** — what systems they're allowed to touch
- **Boundaries** — what needs permission, what's never allowed

The more specific the job description, the better the agent performs.

---

## Step 1: Create a new crew member

Dashboard → **Customize → Mates → New Mate**

| Field | What to put |
|---|---|
| Name | Something job-like, e.g. `ticket-triager` |
| Job description | One specific paragraph. Vague = vague results. |
| Toolbox | Start narrow. Only what the job needs. |
| Trigger words | Short phrases that uniquely call this agent, e.g. "triage this ticket" |

## Step 2: Test what it CAN do

Send a message that matches a trigger word, e.g.:
```
triage this ticket: [paste a real or sample ticket]
```
Confirm it responds as that agent.

## Step 3: Test the boundary

Ask it to do something outside its job on purpose, e.g.:
```
close this ticket for me
```
Watch for an approval prompt, or a refusal. **This is the moment that proves the boundary is real, not just a suggestion.**

## Step 4: See the master switch

Settings → **Agent Autonomy**. Two modes:
- **Supervised** — asks before acting (what you just did)
- **Autopilot** — acts on pre-approved things without asking

Supervised is the safer starting point while your team is still learning what to trust an agent with.

---

## Quick Reference — All 3 Modules

| Task | Where |
|---|---|
| Sign in | `kiro-cli login` |
| Check setup | `kirocrew doctor` |
| See runtime status | `kirocrew status` |
| Open dashboard | `http://localhost:5476` |
| Add a scheduled job | `kirocrew cron add ...` or Schedule panel |
| Run a job now | `kirocrew cron trigger <id>` |
| Create a crew member | Customize → Mates → New Mate |
| Control how much an agent can do without asking | Settings → Agent Autonomy |

That's all 3 modules. ← [Back to all modules](README.md)
