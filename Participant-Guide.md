# Kiro Crew Training — Participant Workbook

**For:** DBP Developers
**Tool:** Kiro Crew (dashboard + CLI)
**Format:** Follow along live — keep this open next to the dashboard

---

## Before You Start

- [ ] You have a Kiro account and can sign in (`kiro-cli login`)
- [ ] You can reach the Crew dashboard (`http://localhost:5476`, or the shared URL your trainer gives you)
- [ ] You have a real project folder to point it at during the labs

**The one idea to keep in mind the whole session:** think of the AI as an associate-level team member. You give it a job description, a toolbox, and boundaries — just like onboarding a new hire.

---

## Part 1 — Account & Token Management

### Step 1: Sign in
```
kiro-cli login
```

### Step 2: Check your setup
```
kirocrew doctor
```
This confirms your login and config are valid.

### Step 3: See who is running
```
kirocrew status
```
Shows the gateway's runtime stats — including whose Kiro plan is funding requests.

### Step 4: Open your account view
Dashboard → **Settings → Account**

**Good to know:** there is no separate Crew subscription. Every request Crew makes to the model — chat, a scheduled job, a Task Runner step — counts against your normal Kiro plan. A scheduled job that only runs a script or shell command (no model call) costs nothing.

---

## Part 2 — Kiro Crew: How It Works

### Think of it this way
Kiro Crew is **one always-on process** (the Gateway). The dashboard, the CLI, the desktop app, and Slack/Discord are all just windows into that same process. Closing the dashboard tab does not stop anything running inside it.

### Step 1: Ask it to read something real
In the dashboard chat, ask it to summarize a real file from your own project — not a toy example. Watch it actually open and read the file.

### Step 2: Set up a scheduled job
Go to the **Schedule** panel (or run the command below) and create a recurring job:
```
kirocrew cron add --cron "0 9 * * 1" "summarize my open DBP tasks"
```
This runs every Monday at 9am.

### Step 3: Run it now, don't wait for Monday
```
kirocrew cron list
kirocrew cron trigger <id>
```
Watch the result come back immediately.

### Step 4: Notice what it does NOT know
Ask it something about a system it has no tool for connected (e.g. "check our production AWS account"). Notice it either gives a generic answer or tells you it has no tool wired in for that. **Capability only exists if the right tool/credential is actually connected — it is not automatic.**

**Good to know:** Crew's security is 8 layers deep (approval prompts, denied commands, sandboxing, audit logging, and more) — every action is logged, not just trusted silently.

---

## Part 3 — Agent Creation

### Think of it this way
A custom agent (a "crew member") is like **onboarding a new associate-level hire**. You don't hand a new hire the keys to everything day one. You give them:
- A **job description** — what they're there to do
- A **toolbox** — what systems they're allowed to touch
- **Boundaries** — what needs permission, what's never allowed

The more specific the job description, the better the agent performs.

### Step 1: Create a new crew member
Dashboard → **Agents / Crew Members** → **Create new**

| Field | What to put |
|---|---|
| Name | Something job-like, e.g. `ticket-triager` |
| Job description | One specific paragraph. Vague = vague results. |
| Toolbox | Start narrow. Only what the job needs. |
| Trigger words | Short phrases that uniquely call this agent, e.g. "triage this ticket" |

### Step 2: Test what it CAN do
Send a message that matches a trigger word, e.g.:
```
triage this ticket: [paste a real or sample ticket]
```
Confirm it responds as that agent.

### Step 3: Test the boundary
Ask it to do something outside its job on purpose, e.g.:
```
close this ticket for me
```
Watch for an approval prompt, or a refusal. **This is the moment that proves the boundary is real, not just a suggestion.**

### Step 4: See the master switch
Settings → **Agent Autonomy**. Two modes:
- **Supervised** — asks before acting (what you just did)
- **Autopilot** — acts on pre-approved things without asking

Supervised is the safer starting point while your team is still learning what to trust an agent with.

---

## Quick Reference

| Task | Where |
|---|---|
| Sign in | `kiro-cli login` |
| Check setup | `kirocrew doctor` |
| See runtime status | `kirocrew status` |
| Open dashboard | `http://localhost:5476` |
| Add a scheduled job | `kirocrew cron add ...` or Schedule panel |
| Run a job now | `kirocrew cron trigger <id>` |
| Create a crew member | Agents / Crew Members → Create new |
| Control how much an agent can do without asking | Settings → Agent Autonomy |

---

*Keep this workbook open during the session. Ask your trainer if a step on screen doesn't match what's written here — the product updates between when this was written and when you're reading it.*
