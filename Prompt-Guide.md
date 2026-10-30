# Kiro Crew — Prompt Guide for DBP

Ready-to-use prompts for each training topic. Copy, paste into the dashboard chat or CLI, adjust the bracketed parts.

---

## 1. Account & Token Management

```
Check my Kiro Crew login status and tell me whose account is funding requests on this gateway.
```

```
Walk me through revoking and re-creating my API key safely.
```

---

## 2. Kiro Crew — How It Works

```
Summarize [FILE PATH] and tell me the 3 most important open items in it.
```

```
Set up a recurring job that runs every [DAY] at [TIME] and summarizes my open [PROJECT] tasks.
```

```
List everything you would need — tools or credentials — to check the health of our [SYSTEM NAME], and tell me plainly if you can already do it or not.
```

---

## 3. Agent Creation

```
Create a new crew member named [NAME]. Its job: [ONE-SENTENCE JOB DESCRIPTION].
Give it read-only access to [SCOPE]. It should respond when a message contains the words: [TRIGGER WORDS].
It must never [THE ONE THING IT SHOULD NEVER DO].
```

```
Ask [CREW MEMBER NAME] to [TASK WITHIN ITS JOB]. Then ask it to [TASK OUTSIDE ITS JOB] and tell me what happens.
```

```
Show me the current permission rules for [CREW MEMBER NAME], and explain which ones are "ask first" vs "never allowed."
```

---

## General-purpose starter prompts (good for day-to-day work)

```
Read [FILE OR FOLDER] and tell me what's outdated, risky, or missing.
```

```
Here is a correction: [WHAT YOU WANT IT TO DO DIFFERENTLY NEXT TIME]. Remember this going forward.
```

```
Before you do [ACTION], tell me what you're about to change and wait for my confirmation.
```

---

*Pair this with `Facilitator-Guide.md` and `Participant-Guide.md` in this repo for the full training flow.*
