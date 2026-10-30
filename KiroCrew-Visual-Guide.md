# Kiro Crew — Visual Guide

**Purpose:** see the shape of Kiro Crew before you explain it out loud. Pairs with `KiroCrew-Cheatsheet.md` (commands) and `Facilitator-Guide.md` (session run-sheet) in this folder.
**Rendering note:** the diagrams below are Mermaid — they render natively in GitHub, VS Code/Kiro IDE preview, and most modern markdown viewers.

---

## 1. The basic shape: one Gateway, many surfaces

```mermaid
flowchart TD
    subgraph Surfaces["How you reach it"]
        A1[Desktop app]
        A2[Web dashboard]
        A3[CLI]
        A4[Slack / Discord / Teams / etc.]
    end

    G["Gateway<br/>(the always-on process)<br/>sessions · memory · schedules · approvals · apps"]

    subgraph Brain["Agent sessions"]
        B1["ACP runtime"]
        B2["Selected harness (kiro-cli by default)"]
        B3["MCP tools"]
        B4["Model (Claude, open-weight, etc.)"]
    end

    Surfaces --> G --> Brain
```

**One thing to say out loud:** the Gateway is the actual product. Everything in "Surfaces" is just a window into the same underlying process, memory, and state. Closing the dashboard doesn't stop anything — the Gateway keeps running.

---

## 2. Inside the Gateway: a 4-role software delivery team

DBP's crew is modeled as a mini SDLC team: **PM → Developer → QA → Reviewer**. Each is a separate crew member (own workspace, own memory, own triggers); the conductor routes each message to whichever one the wording matches.

```mermaid
flowchart LR
    U["You (or a teammate)<br/>ask a question"] --> C{"Conductor<br/>(kirocrew-conductor)"}

    C -->|"trigger: 'requirements',<br/>'scope', 'acceptance criteria'"| M1["PM<br/>turns the ask into scope<br/>+ acceptance criteria"]
    C -->|"trigger: 'implement',<br/>'build', 'fix the bug'"| M2["Developer<br/>implements against<br/>that scope"]
    C -->|"trigger: 'test this',<br/>'write tests', 'find bugs'"| M3["QA<br/>writes/runs tests,<br/>reports defects"]
    C -->|"trigger: 'review this',<br/>'is this ready', 'approve'"| M4["Reviewer<br/>final gate before<br/>it ships"]
    C -->|no trigger matches| D["Default agent<br/>handles it generically"]

    M1 -. hands off scope to .-> M2
    M2 -. hands off build to .-> M3
    M3 -. hands off verified work to .-> M4
```

**One thing to say out loud:** PM, Developer, QA, and Reviewer are **config, not separate brains** — same underlying model unless you deliberately pin a different one per role. What makes them different is the workspace, memory, instructions, and tools each is bound to, and the trigger wording that routes a message to the right one. Pick trigger phrases that don't overlap — "test this" (QA) and "review this" (Reviewer) are deliberately distinct, not two words for the same thing.

---

## 3. Where real capability actually comes from (it's not automatic)

```mermaid
flowchart TD
    Ask["'Developer, deploy this and check CloudWatch'"] --> Member["Developer crew member"]
    Member --> HasTools{"Does this member<br/>have AWS tools wired in?"}
    HasTools -->|No| Generic["Generic, non-specific answer<br/>— it never touched AWS"]
    HasTools -->|"Yes: shell + AWS CLI credentials,<br/>or an AWS MCP server"| Real["Real answer — it actually<br/>queried CloudWatch / Security Hub / etc."]
    Real --> Creds["⚠ Needs real IAM credentials<br/>reachable by this member —<br/>scope them least-privilege, read-only where possible"]
```

**One thing to say out loud:** this is the single most common false assumption people bring into this training. Flag it early. Note the 4-role team above has no dedicated DevOps/SRE member — if DBP wants real AWS deployment/troubleshooting handled by a specialist rather than bolted onto Developer, that's a 5th role to add later, not something these four give you by default.

---

## 4. Laptop vs. hosted: the deployment decision

```mermaid
flowchart LR
    subgraph Laptop["Option A — runs on your laptop"]
        L1["Only reachable while<br/>your laptop is on and awake"]
        L2["Cron jobs / scheduled tasks<br/>pause when laptop sleeps"]
        L3["Simplest to set up"]
    end

    subgraph Hosted["Option B — runs on an always-on host<br/>(VPS, shared server, or kirocrew cloud launch → EC2 in your own AWS account)"]
        H1["Reachable 24/7"]
        H2["Cron jobs and Task Runner<br/>keep working unattended"]
        H3["Reachable by a team via<br/>Slack / dashboard link / tailnet"]
    end

    Laptop -. "move state with kirocrew snapshot / restore" .-> Hosted
```

---

## 5. The part people miss: what "sharing" actually shares

```mermaid
flowchart TD
    Gateway["One Gateway<br/>(hosted on a shared server)"]

    Gateway --> Memory["ONE shared memory<br/>— not per-person"]
    Gateway --> Config["ONE shared config<br/>— not per-person"]
    Gateway --> KiroAccount["ONE kiro-cli login<br/>= ONE Kiro subscription funding<br/>EVERY request from EVERYONE"]
    Gateway --> Trust["FLAT trust<br/>— no roles, no per-user ACLs<br/>(Shared Crew access-control is proof-of-concept only)"]

    Alice["Alice (Slack)"] --> Gateway
    Bob["Bob (dashboard link)"] --> Gateway
    Carol["Carol (phone)"] --> Gateway

    KiroAccount -.->|"charges land here,<br/>not on Alice/Bob/Carol's<br/>own personal subscriptions"| Owner["Whoever's account<br/>is logged into kiro-cli<br/>on this host"]
```

**One thing to say out loud:** sharing the link is easy. Sharing it *safely and with the right billing owner in mind* is the actual decision DBP needs to make, not a technical step.

---

## 6. Decision guide: one shared Crew, or one Crew per person?

```mermaid
flowchart TD
    Start{"What does DBP actually need?"}

    Start -->|"Each person wants their own<br/>private assistant, own memory,<br/>own subscription usage tracked"| PerPerson["✅ One Crew per person<br/>Each runs their own Gateway + own kiro-cli login"]

    Start -->|"A team wants ONE shared tool<br/>(e.g. shared on-call Support bot)<br/>and is OK with shared memory +<br/>one account footing the bill"| Shared["✅ One hosted, shared Crew<br/>One clearly-owned account funds it<br/>Everyone who can reach it has equal access — no exceptions"]

    Start -->|"Need per-user billing AND<br/>per-user access control on<br/>ONE shared instance"| NotYet["❌ Not supported yet<br/>Shared Crew's per-person attribution/limits<br/>are proof-of-concept, not functional in this build"]
```

---

## 7. Quick summary table

| Question | Answer |
|---|---|
| Can one Crew be reached by multiple people? | Yes — via Slack/Discord/Teams, a shared dashboard link, or a tailnet |
| Does each person's own Kiro subscription get used? | **No.** One `kiro-cli` login funds every request, regardless of who sent it |
| Is there access control between users on one shared Crew? | **No roles, no per-user ACLs today.** Everyone who can reach it has equal rights |
| Is memory/config shared or separate per person? | **Shared.** One memory store, one config, for everyone on that instance |
| Does it know how to troubleshoot AWS out of the box? | **No.** Only if a member has AWS CLI/credentials or an AWS MCP server explicitly wired in |
| Best setup for "everyone has their own AI assistant"? | One Crew per person, not one shared instance |
| Best setup for "the team wants one shared on-call bot"? | One hosted shared Crew, with one clearly chosen, properly-sized paid account behind it |