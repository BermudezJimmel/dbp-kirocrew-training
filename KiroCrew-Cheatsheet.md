# Kiro Crew — Running It: Cheat Sheet

**Source:** verified against the actual `kirocrew` CLI module spec in [kirodotdev/KiroCrew](https://github.com/kirodotdev/KiroCrew) (`docs/system-specs/modules/cli.md`, `install.md`, `README.md`, `taskrunner.md`, `security.md`, `computer-use.md`), not just blog posts.
**Audience:** you, running/demoing Kiro Crew for the DBP training. Companion to `Facilitator-Guide.md` / `Participant-Guide.md` in this same folder.

---

## 1. Install

| Goal | Command |
|---|---|
| Fastest install (signed wheel, no repo clone) | `curl -fsSL https://download.crew.kiro.dev/cli.sh \| sh` |
| Pin a specific version | `curl -fsSL https://download.crew.kiro.dev/cli.sh \| sh -s -- --version 0.6.0` (min pinnable: `0.1.2`) |
| Docker (always-on server) | `docker run -d --name kirocrew -p 127.0.0.1:5476:5476 -v kirocrew-home:/home/kirocrew ghcr.io/kirodotdev/kirocrew:stable` |
| Build from source | `git clone https://github.com/kirodotdev/KiroCrew.git && cd KiroCrew && make build && source .venv/bin/activate` |
| Desktop app | Download `.dmg` / `.exe` / `.deb` / `.rpm` / AppImage from the [releases page](https://github.com/kirodotdev/KiroCrew/releases) |

**Prerequisite:** the default backend runs on `kiro-cli`. Install it separately and run `kiro-cli login` **before** starting Crew — first launch checks for this and tells you if it's missing.

---

## 2. First-time setup

```bash
kirocrew setup       # installs agent config, saves project dir, configures credentials
kirocrew doctor      # verifies kiro-cli is installed and config is valid
kirocrew gateway     # starts the server (dashboard + messaging channels)
```

| Variant | What it does |
|---|---|
| `kirocrew setup --agent-only` | Only install agent config, skip credentials |
| `kirocrew setup --slack` | Guided Slack credential + slash-command setup |
| `kirocrew setup --whatsapp` | Guided WhatsApp pairing/opt-in |
| `kirocrew doctor && kirocrew gateway` | Standard "verify then start" sequence |

Then open **http://localhost:5476** — this is the **only port Crew opens**; messaging channels (Slack, Discord, etc.) connect outbound, nothing else listens.

---

## 3. Starting / stopping the Gateway

| Command | What it does |
|---|---|
| `kirocrew gateway` | Foreground — dies when you close the terminal |
| `kirocrew gateway --slack-only` | No dashboard, no SSH tunnel instructions |
| `kirocrew gateway --no-crons` | Skip the cron scheduler (use if another instance already handles crons) |
| `kirocrew gateway --no-tunnel` | Never publish a remote tunnel for this process |
| `kirocrew stop` | Stop a running gateway (service-aware: stops the OS service if installed, otherwise kills the process on the dashboard port) |
| `kirocrew restart [--port PORT]` | Restart the running gateway |
| `kirocrew status` | Show runtime stats from the running gateway |
| `kirocrew gateway-pid` | Print the gateway's PID |
| `kirocrew token` | Print a dashboard access URL with an auth token baked in |
| `kirocrew open [SESSION] [--print]` | Open the desktop app on a specific session, or just bring it forward |

**Run it as a persistent background service** (survives SSH disconnects, restarts on crash, starts on boot):

```bash
kirocrew service install     # systemd unit (Linux, sudo for this step only) or launchd agent (macOS, no sudo)
kirocrew service status      # check it
kirocrew service uninstall   # remove it
```

Only one gateway runs at a time — `gateway` (foreground) and `service install` (background) are mutually exclusive ways to run the same thing, not two separate instances.

---

## 4. Logs & troubleshooting

| Command | What it does |
|---|---|
| `kirocrew logs` | Tail gateway logs (systemd journal / launchd stdout / `~/.kiro/crew/gateway.log`, whichever applies) |
| `kirocrew logs -f` | Follow logs live |
| `kirocrew doctor` | Health check: kiro-cli install, config validity, MCP server reachability |
| `kirocrew security events` | Recent audit events from the security event log (SEL) |
| `kirocrew security verify` | Verify the audit log's integrity (HMAC chain) |
| `kirocrew security deny-list` | Show currently active deny patterns |

---

## 5. Chat (interactive, you're watching)

| Command | What it does |
|---|---|
| `kirocrew chat -m "message"` | Send one message, print the streamed response, exit |
| `kirocrew chat` | Interactive chat (readline), exit with Ctrl+D |
| `kirocrew chat --model X` | Override the model for this session |
| `kirocrew chat --agent <name>` | Chat as a specific crew member/agent instead of the default |

---

## 6. Task Runner — give it a spec and walk away

This is the "unattended" capability — give it a markdown spec file, it plans the steps, executes each in an isolated session, runs tests, retries failures, checkpoints progress, and commits/reverts via git per step.

```bash
kirocrew run TASK.md
```

**Know this before you demo it:**
- **Tool approval during a headless `kirocrew run` defaults to deny.** There's no interactive prompt in a terminal-only run — without an approval callback, any tool call that isn't already allowed by a hook rule is rejected (`headless_no_authorization`). This is a deliberate fail-closed design, not a bug.
- From the **dashboard**, a run can opt into a scoped, time-boxed **auto-approve** trust toggle — default **off**, must be explicitly turned on per run, and it's audited and expires. Deny-list / sensitive-path blocks still apply even when auto-approve is on.
- Good live-demo spec: small and self-contained (your own Facilitator Guide already recommends this — don't let anyone hand it an ambitious multi-file spec live).

---

## 7. Scheduling (cron)

```bash
kirocrew cron add --every 1h "check for new PRs"          # recurring, every N
kirocrew cron add --cron "0 9 * * 1-5" "daily summary"    # cron syntax, timezone-aware
kirocrew cron add --at "2026-11-05T09:00" "one-shot reminder"
kirocrew cron list
kirocrew cron remove <id>
kirocrew cron pause/resume/trigger <id>
kirocrew cron preview <script>
```

A job can be a prompt, a `--script FILE.py:FUNC` (zero-token, runs no model), or a `--command SHELL`. A job that keeps failing auto-pauses after N consecutive failures (configurable, default behavior backs off with increasing delay).

---

## 8. Memory & self-learning

| Command | What it does |
|---|---|
| `kirocrew memory show [preferences\|projects\|history]` | View what Crew remembers |
| `kirocrew memory list/search/stats/audit` | Inspect memory contents and usage |
| `kirocrew memory export/import/migrate` | Move memory between stores — **this is the exact mechanism Kiro IDE uses to import Crew's memory** |
| `kirocrew memory backup/backups/restore` | Back up / restore a memory store |
| `kirocrew memory forget <key>` | Delete a specific memory entry |
| `kirocrew learn add/list/remove` | Manage corrections it has learned as durable lessons |
| `kirocrew consolidate [session_key] [--all]` | Force history consolidation (triggers skill extraction from repeated patterns) |

---

## 9. Delegation (subagents) & crew members

| Command | What it does |
|---|---|
| `kirocrew spawn run/list` | Spawn an isolated background subagent for parallel work; `list` shows pending/waiting spawns too |
| `kirocrew agent list/create/update/delete/reset-model` | Manage named crew members/agents (each binds a kiro-cli agent template + workspace + memory store + model) |

A **crew member** ("mate"/"crew") is a config entry, not a separate install: name + workspace + memory store + model + `triggers` that tell the conductor when to route work to it. This is the real mechanic behind "Kiro Crew routes work to the right specialist."

---

## 10. Apps (App Kit)

| Command | What it does |
|---|---|
| `kirocrew app init <name> [--backend] [--ui] [--cron]` | Scaffold a new app |
| `kirocrew app install/list/enable/disable/uninstall` | Manage installed apps (live-applied to a running gateway) |
| `kirocrew app uninstall NAME --purge-data` | Uninstall and permanently delete its data |
| `kirocrew app dev <name> [--off]` | Toggle dev mode (live reload) for an installed app |

---

## 11. Computer Use (desktop/browser control) — off by default

```bash
kirocrew computer doctor [--json]     # check availability
kirocrew computer apps                # list enumerable on-screen apps
kirocrew computer call <tool> [k=v]   # invoke a computer-use tool manually (debugging)
```

**Governance is deliberately simple, not granular:** one operator-controlled on/off switch in a file the agent itself can neither read nor write, ANDed with one capability flag. There is no per-app or per-action permission layer on top of that — it's on or it's off. Confirm with DBP's security contact before enabling this for a demo; default to leaving it off.

---

## 12. Backup / restore (whole Crew state)

```bash
kirocrew snapshot [output_dir]              # .tar.gz of all Crew state
kirocrew snapshot --list
kirocrew snapshot --keep 7                  # auto-prune to N most recent
kirocrew restore <file>                     # auto-detects replace vs merge
kirocrew restore <file> --dry-run           # preview only, writes nothing
kirocrew restore <file> --components X,Y    # selective restore
```

Useful before a live demo: snapshot your known-good state first, so a bad demo moment is a `restore --dry-run` + `restore` away from undone.

---

## 13. Telemetry

```bash
kirocrew telemetry status      # see exactly what the daily anonymous heartbeat sends
kirocrew telemetry disable     # turn it off, persists across restarts
```

Per the project's own README: never sends prompts, model responses, file contents, paths, repo/branch names, credentials, hostname, username, or IP. Five fields only (random instance id, app version, Python minor version, install path, first-send flag), at most once a day.

---

## 14. Config

```bash
kirocrew config get [key]            # full effective config, or one dot-path value
kirocrew config set <key> <value>    # auto-detects bool/int/float/JSON/string
kirocrew config edit                 # open config.json in your editor
```

Data lives under `~/.kiro/crew` by default; override with `KIROCREW_HOME`.

---

## Quick reference: the "just get it running for a demo" path

```bash
curl -fsSL https://download.crew.kiro.dev/cli.sh | sh    # install
kiro-cli login                                           # sign in the backend first
kirocrew setup                                            # configure
kirocrew doctor                                           # verify
kirocrew gateway                                           # start (foreground, for a live demo)
# open http://localhost:5476
```

To make it persistent instead of a foreground demo process: `kirocrew service install` in place of the last line.