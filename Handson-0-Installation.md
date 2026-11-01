# Installation Guide — Get Kiro Crew Running

**Do this before the session starts.** Next: [Module 1 — Account & Token Management](Handson-1-Account-Token.md)

---

## Step 1: Download

Open the [Kiro Crew Downloads page](https://github.com/kirodotdev/KiroCrew/releases/latest) and click the file for your OS:

| Your OS | File to click |
|---|---|
| Windows | the file ending in **`Setup-x64.exe`** |
| Mac — Apple Silicon (M1/M2/M3/M4) | the file ending in **`arm64.dmg`** |
| Mac — Intel | the file ending in **`x64.dmg`** |
| Linux | the **`.AppImage`**, **`.deb`**, or **`.rpm`** file |

## Step 2: Install it

- **Windows:** double-click the `.exe`, click through the installer.
- **Mac:** double-click the `.dmg`, drag Kiro Crew into Applications.
- **Linux:** make the `.AppImage` executable and run it, or `dpkg -i` the `.deb` / `rpm -i` the `.rpm`.

## Step 3: Open it, sign in

Open **Kiro Crew** from your Start Menu / Applications, like any other app.

First launch shows a **Set up Kiro** page. Follow it — it signs you in and finishes setup for you. No terminal needed.

## Step 4: Check it worked

Open a terminal just for this one check:
```
kirocrew doctor
```
Green checks across the board = ready for Module 1.

---

**Prefer the terminal?** One line does everything above at once:
```
curl -fsSL https://download.crew.kiro.dev/cli.sh | sh
kirocrew setup
```

Next: [Module 1 — Account & Token Management](Handson-1-Account-Token.md)
