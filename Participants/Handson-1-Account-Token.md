
# Hands-On 1 — Account & Token Management

**Module 1 of 3** · [All modules](README.md) · Previous: [Installation](Handson-0-Installation.md) · Next: [Module 2 — How It Works](Handson-2-How-It-Works.md)

---

## Before You Start

- [ ] You have a Kiro account and can sign in (`kiro-cli login`)
- [ ] You can reach the Crew dashboard (`http://localhost:5476`, or the shared URL your trainer gives you)

**The one idea to keep in mind:** think of the AI as an associate-level team member. You give it a job description, a toolbox, and boundaries — just like onboarding a new hire.

---

## Step 1: Sign in

```
kiro-cli login
```

## Step 2: Check your setup

```
kirocrew doctor
```
This confirms your login and config are valid.

## Step 3: See who is running

```
kirocrew status
```
Shows the gateway's runtime stats — including whose Kiro plan is funding requests.

## Step 4: Open your account view

Dashboard → **Settings → Account**

**Good to know:** there is no separate Crew subscription. Every request Crew makes to the model — chat, a scheduled job, a Task Runner step — counts against your normal Kiro plan. A scheduled job that only runs a script or shell command (no model call) costs nothing.

---

Done with Module 1? → [Module 2 — How It Works](Handson-2-How-It-Works.md)
