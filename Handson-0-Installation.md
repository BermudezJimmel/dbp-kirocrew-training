# Installation Guide — Get Kiro Crew Running

**Do this before the session starts.** Next: [Module 1 — Account & Token Management](Handson-1-Account-Token.md)

---

## macOS / Linux

### Step 1: Open Terminal, paste this once

```
curl -fsSL https://download.crew.kiro.dev/cli.sh | sh
```

This downloads the real installer for your machine and runs it. It also installs `kiro-cli`, the engine Crew runs on.

### Step 2: Run the setup wizard

```
kirocrew setup
```

Answer its questions (workspace folder, timezone, sign-in).

### Step 3: Get the app icon (macOS)

```
kirocrew setup --electron-only
```

Now open **Kiro Crew** from Applications, like any other app. No terminal needed day to day — the icon opens the dashboard in its own window.

## Windows

Kiro Crew on Windows runs from a Python source install (`python -m kiro_crew gateway`), not a one-click installer yet. Ask your trainer before the session if you're on Windows.

## Check it worked

```
kirocrew doctor
```

Green checks across the board = ready for Module 1.

---

Next: [Module 1 — Account & Token Management](Handson-1-Account-Token.md)
