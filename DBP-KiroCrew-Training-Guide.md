# Kiro Crew Training — Comprehensive Guide
**Client:** DBP
**Delivered by:** Sagesoft Solutions Inc.
**Audience:** Developers (20 participants)
**Format:** 2 hours total — 15-20 min discussion, then hands-on for the remainder
**Tool in focus:** Kiro Crew (the always-on agent orchestration layer, separate from Kiro IDE)

---

## 1. Session Objective

By the end of this session, every participant can:
1. Explain what Kiro Crew is and how it differs from a normal AI chat session.
2. Create and manage an account token so the Crew gateway can run under their identity.
3. Create a custom agent ("crew member") scoped to a real job, with its own tools and boundaries.
4. Set up at least one scheduled job (cron) and watch it run.

The three required topics map onto the agenda below:

| Client request | Covered in |
|---|---|
| Account Token Management | Module 1 |
| Kiro Crew — how it works, how to use it in our workflow | Discussion + Module 2 (Scheduling/Task Runner) |
| Agent Creation — create, configure, automate tasks | Module 3 |

---

## 2. The Teaching Frame

Use one analogy for the whole session: **treat the AI like an associate-level / individual-contributor hire, not a magic wand.**

- You give it a **job description** (the prompt / instructions).
- You give it a **toolbox** (which tools and MCP servers it can touch).
- You set **boundaries** (what it must ask permission for, what it can never do).

Vague onboarding → vague results. The more specific the job description, the better the output. This frame carries through tokens (its ID badge), agent creation (its job description), and Crew (hiring someone who works unsupervised, on a schedule).

---

## 3. Session Flow (120 minutes)

| Time | Duration | Segment |
|---|---|---|
| 0:00 – 0:20 | 15-20 min | Discussion: what Kiro Crew is, the associate-hire frame, expectations |
| 0:20 – 0:45 | 25 min | Hands-On Module 1: Account & Token Management |
| 0:45 – 1:25 | 40 min | Hands-On Module 2: Kiro Crew — How It Works + Live Demo |
| 1:25 – 2:00 | 35 min | Hands-On Module 3: Agent Creation |

Rule for the room: after minute ~20, nobody just listens. Everyone has the Crew dashboard open and does something.

---

## 4. Discussion Segment (15-20 min, talking points only)

1. **What Kiro Crew is** (5 min) — a persistent, open-source, always-on orchestration layer. One Gateway process, reachable from the dashboard, CLI, desktop app, or chat apps (Slack, Discord, Teams). Closing the dashboard tab does not stop it.
2. **The associate-hire frame** (5 min) — introduce the analogy above.
3. **Why these three topics** (5 min):
   - **Account/token management** — how the Crew gateway authenticates and what funds every request it makes.
   - **How Crew works** — one Gateway, many surfaces, work that keeps going while you are away (Task Runner, Scheduling, Webhooks).
   - **Agent Creation** — a crew member is config (workspace + memory + model + trigger words), not a separate brain.
4. **Expectation-setter** (3-5 min): Crew can control a browser or desktop directly if explicitly turned on. Off by default — flag that DBP's security team should sign off before anyone enables it.
5. **Room check** (2 min) — confirm everyone has the dashboard open, signed in, and a project folder picked.

---

## 5. Module 1 — Account & Token Management (25 min)

### Core facts
- Kiro Crew needs a Kiro account. Sign-in happens once, via `kiro-cli login`, **before** the gateway starts. First launch checks for this and tells you if it is missing.
- There is **no separate Crew subscription**. Every agent request Crew makes — chat, a scheduled job, a Task Runner step — bills the same Kiro plan as the IDE or CLI.
- A `script` or `command` cron job that runs no model call uses **zero tokens**. Only jobs that invoke the agent (a prompt-based job) count against usage.
- Token-safety note for the room: treat any API key or login token the same as a password — the gateway doc warns the same way.

### Hands-on steps
1. Open a terminal. Run `kiro-cli login` if not already signed in.
2. Run `kirocrew doctor` — confirms kiro-cli is installed and config is valid.
3. Run `kirocrew status` — shows the running gateway's runtime stats, including whose account is funding it.
4. Open the dashboard (`http://localhost:5476`) → **Settings → Account** (or run `kirocrew token` to get a dashboard URL with an auth token baked in).
5. Discussion prompt for the room: *"If we put one shared Crew gateway on a server for the whole team, whose Kiro plan pays for every request everyone sends?"* — there is no per-user billing split today; one account funds all traffic through a shared instance.

---

## 6. Module 2 — Kiro Crew: How It Works (40 min)

### Core facts (say these out loud, point at the diagrams in `KiroCrew-Visual-Guide.md`)
- **One Gateway, many surfaces.** Dashboard, CLI, desktop app, Slack/Discord/Teams are all windows into the same process, memory, and state.
- **It keeps working while you are away.** Three mechanisms:
  - **Cron / Scheduling** — timezone-aware recurring jobs (`kirocrew cron add --every 1h "..."`, or `--cron "0 9 * * 1-5"`).
  - **Task Runner** — hand it a markdown spec (`kirocrew run TASK.md`), it plans the steps, executes each in an isolated session, runs checks, checkpoints progress.
  - **Webhooks / Heartbeat** — external events (a failed CI build, a PR update) wake a session automatically.
- **Security is layered, not a single switch.** 8 layers: owner lock, denied-command patterns, a governance ceiling, sensitive-path protection, tool approval, input validation, OS sandboxing, output/credential redaction — all audit-logged.
- **It does not know your infrastructure by default.** A crew member only gets real answers about AWS, your repo, etc. if it has the matching tool/credentials wired in (shell + AWS CLI, or an MCP server). Otherwise it gives a generic, non-specific answer.

### Live demo plan (do this live, in front of the room)
1. Open the dashboard. Point at the **Projects** panel.
2. Ask it, in chat, to **summarize a real file from your own DBP project folder** — this is the fastest way to feel "it reads my actual files," not a toy example.
3. In the **Schedule** panel, create one real recurring job: *"every Monday 9am, summarize my open DBP tasks."* Confirm it is listed.
4. Trigger it once immediately with `kirocrew cron trigger <id>` (or the dashboard's **Run now**) so the room sees a result without waiting until Monday.
5. Narrate what just happened using the layers above: one Gateway, a scheduled job, a real file read, no model call wasted on anything else.

### Discussion prompt
*"Crew works unsupervised, on a schedule — is that a hire you want on the team today, or one you'd want IT/security to vet first?"*

---

## 7. Module 3 — Agent Creation (35 min)

### Core facts
- A **crew member** ("agent") is a **config entry**, not a separate model or brain: name + workspace + memory store + model + **trigger words** that tell the conductor when to route a message to it.
- Three parts of the job description:
  - **Prompt/instructions** — the job description itself. Vague instructions → vague results.
  - **Tools** — the toolbox. What it can even see (read, write, shell, specific MCP servers).
  - **Permissions / approval rules** — the boundaries. A `deny` rule always wins over anything else.
- The conductor routes each incoming message to whichever crew member's trigger words match. Pick non-overlapping trigger phrases (e.g. "test this" vs. "review this") so routing is unambiguous.

### Hands-on steps
1. In the dashboard, open **Agents / Crew Members** → **Create new**.
2. Fill in:
   - **Name:** something job-like, e.g. `ticket-triager`.
   - **Job description (prompt):** one paragraph, specific. Example: *"You triage incoming support tickets. Read the ticket, classify severity, and suggest an owner. Never close or reassign a ticket yourself — only recommend."*
   - **Tools:** start narrow — read-only access to the ticket source.
   - **Trigger words:** short phrases that uniquely identify when this member should handle a message, e.g. "triage this ticket."
3. Save, then send it a test message that matches a trigger word. Confirm the conductor routed it correctly.
4. Send it a message that asks it to do something outside its stated job (e.g. "close this ticket"). Watch it ask for approval or refuse — this is the moment that proves the boundary is real, not just a suggestion.
5. Discussion prompt: *"What's one task on your team you'd trust an associate-level hire with completely unsupervised? What's one you wouldn't?"* — this maps directly onto what should be an `allow` vs. a `deny` rule.

---

## 8. Wrap-up & Q&A (fold into the last few minutes of Module 3)

Recap what each participant did hands-on:
- Signed in and checked their account/token status.
- Watched a real scheduled job read a real file and report back.
- Created a custom crew member with its own job description, toolbox, and boundary, and tested both sides of that boundary.

Open floor, then close.

---

## 9. Trainer Checklist Before Delivery Day

- [ ] Confirm every participant has `kiro-cli` installed and can sign in before the session starts.
- [ ] Run the full demo (Module 2, step 1-4) once end-to-end on the actual delivery machine/network.
- [ ] Pick one real, small, non-sensitive DBP file to summarize live — do not use a file with client-confidential data on the projector.
- [ ] Confirm with DBP in writing whether Browser/Computer Use may be mentioned or demoed; default to NOT demoing it.
- [ ] Have `Participant-Guide.md` and the slide deck (`DBP-KiroCrew-Presentation.html`) ready.
- [ ] Snapshot your own Crew state (`kirocrew snapshot`) before the session, so a bad live-demo moment is one `restore` away from undone.

---

*Prepared for Sagesoft Solutions Inc. Re-verify commands and UI labels against the installed Kiro Crew build before delivery — the product updates frequently.*
