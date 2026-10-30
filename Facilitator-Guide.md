# Kiro Crew Training — Facilitator Guide

**Client:** DBP
**Delivered by:** Sagesoft Solutions Inc.
**Format:** 2 hours — 15-20 min discussion, remainder hands-on
**Audience:** Developers (20 participants)
**Tool in focus:** Kiro Crew (dashboard + CLI). Kiro IDE is not covered — Crew is a separate product.
**Companion files:** `Participant-Guide.md` (hand to the room), `KiroCrew-Cheatsheet.md` (your command reference), `KiroCrew-Visual-Guide.md` (diagrams to project), `DBP-KiroCrew-Presentation.html` (slide deck for the discussion segment)

---

## The Core Teaching Frame

Use this analogy throughout, especially for Agent Creation — it's the fastest way to make the permissions model click for developers:

> **A custom crew member is an associate-level hire, not a magic wand.** You write it a job description (the prompt), hand it a toolbox (`tools`), and set boundaries on what it can do without asking (permission rules). The quality of the agent is mostly the quality of the job description — vague instructions get vague results, the same way a vague onboarding leaves a new employee guessing.

Call back to this frame in all three modules: tokens = the hire's ID badge, agent creation = writing their job description, Crew-as-a-whole = hiring someone who works unsupervised, on a schedule, instead of sitting next to you.

---

## Session Flow (120 min)

| Time | Duration | Segment |
|---|---|---|
| 0:00 – 0:20 | 15-20 min | Discussion: what Kiro Crew is, the associate-hire frame, expectations |
| 0:20 – 0:45 | 25 min | Hands-On: Account & Token Management |
| 0:45 – 1:25 | 40 min | Hands-On: Kiro Crew — How It Works + Live Demo |
| 1:25 – 2:00 | 35 min | Hands-On: Agent Creation |

Rule for the room: after minute ~20, nobody is just listening — everyone has the dashboard open and is doing something.

---

## Discussion Segment (15-20 min) — Talking Points Only

Project the slide deck (`DBP-KiroCrew-Presentation.html`) for this segment.

1. **What Kiro Crew is** (5 min) — a persistent, always-on orchestration layer that keeps running whether or not anyone has the dashboard open. One Gateway process; the dashboard, CLI, desktop app, and chat integrations are all just windows into it.
2. **The associate-hire frame** (5 min) — introduce the analogy above. This is the lens for the whole session.
3. **Why these three topics** (5 min):
   - **Account/token management** — the badge: how the gateway authenticates, and whose plan funds every request.
   - **How Kiro Crew works** — the unattended employee: scheduling, Task Runner, webhooks, and what it can and can't do out of the box.
   - **Agent Creation** — writing a job description for a narrow, scoped assistant instead of one general-purpose one.
4. **Expectation-setter** (3-5 min): Crew can control a browser or desktop directly if turned on — off by default, needs DBP's security sign-off. Flag this now so it doesn't surprise anyone later.
5. **Room check** (2-3 min) — confirm everyone is signed in and has the dashboard reachable. Fix problems now, not mid-lab.

---

## Module 1 — Account & Token Management (25 min)

**Participant steps:** `Participant-Guide.md → Part 1`

### What to narrate while they do each step

| Step | What's happening | Note |
|---|---|---|
| `kiro-cli login` | This is their "badge" — how the gateway knows who they are | One-time sign-in, done before the gateway starts |
| `kirocrew doctor` | Health check: confirms login + config are valid | Run this first whenever something seems broken |
| `kirocrew status` | Shows the running gateway's stats | Point out: this also shows whose account is funding requests |
| Dashboard → Settings → Account | In-dashboard account view | Preview this yourself before the session so your narration matches what's on screen |

### Discussion prompt for the room
*"If we put one shared Crew gateway on a server for the whole team, whose Kiro plan pays for every request everyone sends?"* — there is no per-user billing split on a shared instance today; one account funds all traffic through it. This is the single most important thing DBP needs to decide before sharing a gateway across a team.

---

## Module 2 — Kiro Crew: How It Works (40 min)

**Participant steps:** `Participant-Guide.md → Part 2`

### What to narrate

| Point | What's happening |
|---|---|
| One Gateway, many surfaces | Closing the dashboard tab does not stop anything — the Gateway keeps running |
| Scheduling (cron) | Timezone-aware recurring jobs; a script/command job uses zero tokens, a prompt-based job uses the Kiro plan |
| Task Runner | Give it a markdown spec, it plans, executes in isolated sessions, checkpoints, retries |
| Webhooks / Heartbeat | External events (failed build, PR update) can wake a session without anyone asking |
| "Does it know my AWS account?" | **No, not automatically.** Only if a crew member has the matching tool/credential wired in. This is the #1 false assumption people bring into this training — flag it early |
| 8-layer security | Approval prompts, denied commands, sandboxing, audit logging — every action is logged, nothing is just trusted silently |

### Live demo plan (do this live)
1. Dashboard chat: ask it to summarize a real file from a DBP project folder — pick something small and non-sensitive ahead of time.
2. Schedule panel: create one real job — *"every Monday 9am, summarize my open DBP tasks."* Confirm it's listed.
3. Trigger it immediately (`kirocrew cron trigger <id>` or dashboard "Run now") so the room sees a real result without waiting until Monday.
4. Narrate the result using the table above: one Gateway, one scheduled job, a real file read.

### Discussion prompt
*"Crew works unsupervised, on a schedule — is that a hire you want on your team today, or one you'd want IT to vet first?"*

---

## Module 3 — Agent Creation (35 min)

**Participant steps:** `Participant-Guide.md → Part 3`

This is the core of the session. Protect this time block if anything runs long elsewhere.

### What to narrate

| Step | What's happening |
|---|---|
| A crew member is config, not a new brain | name + workspace + memory + model + trigger words — same underlying model unless deliberately pinned otherwise |
| Job description / toolbox / boundaries | Map directly to: prompt, `tools`, permission rules |
| Trigger words | What tells the conductor to route a message to this member — pick non-overlapping phrases |
| The approval moment (Step 3 in the workbook) | **The must-not-skip moment.** Ask the agent to do something outside its job, watch it ask for approval or refuse. This is what makes "boundaries, not suggestions" land. Budget time for it even if you have to cut something else |
| Agent Autonomy (Supervised vs. Autopilot) | The master on/off switch for approval prompts — Supervised is the safer default for a team still learning what to trust |

### Discussion prompt
*"What's one task on your team you'd trust an associate-level hire with completely unsupervised? What's one you wouldn't?"* — maps directly onto what should go in the permission rules as allow vs. deny.

---

## Wrap-up & Q&A (fold into the last ~10 min of Module 3)

- Recap: an account they checked, a scheduled job that read a real file, an agent they wrote and tested — both the "can do" and "can't do" side.
- What's ready to use today vs. what needs DBP's security review (Browser/Computer Use).
- Open floor, then close.

---

## Trainer Checklist Before Delivery Day

- [ ] Confirm every participant can sign in with `kiro-cli login` before the session starts.
- [ ] Run the full Module 2 live-demo sequence once end-to-end on the actual delivery machine/network beforehand.
- [ ] Pick one real, small, non-sensitive DBP file to summarize live — never a file with client-confidential data on the projector.
- [ ] Confirm with DBP in writing whether Browser/Computer Use may be mentioned; default to not demoing it.
- [ ] Snapshot your own Crew state (`kirocrew snapshot`) before the session, so a bad live-demo moment is a `restore` away from undone.
- [ ] Have `Participant-Guide.md` and the slide deck ready to hand out / screen-share.

---

*Prepared for Sagesoft Solutions Inc. Re-verify commands and UI labels against the installed Kiro Crew build before delivery — the product updates frequently.*
