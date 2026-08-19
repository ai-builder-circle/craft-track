# Craft Track

Build a production-grade REST API by hand — CRUD, security, and domain-driven design — with **no AI-generated code.** This repo is the curriculum and the learning materials. You build in your own repo.

> Maintained by FORGE / `ai-builder-circle`. Reference only — nothing to run here.

---

## The path, start to finish

Three different Git actions show up below, and mixing them up is where people stall. Read this once before you touch anything.

- **Use this template** → creates a brand-new repo *you own*, with clean history. You do this once per starter.
- **Clone** → copies *your* repo to your machine so you can work. You always clone the repo the template just made — never the template itself.
- **Fork** → *not used here.* Forks stay tied to an upstream and are for contributing back to it. You're building your own thing, so you don't fork.

### Step 0 — Read first, clone nothing yet

Read [`CURRICULUM.md`](CURRICULUM.md) top to bottom so you know the shape of the sixteen weeks. Then open the concept doc for Week 1 (see [Opening the labs](#opening-the-concept-labs) — the files are interactive, so you open them in a browser, not on GitHub). This repo is for reading. Nobody builds in it.

### Step 1 — Get your own Phase 1 repo

Go to **[elective-craft-track-http](https://github.com/ai-builder-circle/http-by-hand-starter)** and click the green **Use this template → Create a new repository**. Name it `http-by-hand`, owner = you. That makes a new repo you own. Now clone *that* — your copy:

```bash
git clone git@github.com:YOUR-USERNAME/http-by-hand.git
cd http-by-hand
```

Do not clone the template directly and try to push to it — it isn't yours, and it would corrupt the shared starter. Always: template → your repo → clone your repo.

### Step 2 — The build loop

1. Fill in `diagnostic/phase-0-answers.md` — no research, no prep — and send it to a mentor. It calibrates where you're starting.
2. Each week: open that week's concept doc here in the hub, then open the matching `BUILD_ORDER.md` inside your repo (start at `src/main/java/.../server/BUILD_ORDER.md`). It tells you exactly what to write next, in order, with the gate check for each file.
3. Write every line yourself. Run `make test`. Commit in small, compiling steps — your git log is part of what a mentor reviews.
4. Add four lines to `LEARNING.md` every session. The "Unsure" line gets read first.

The docs map straight onto the weeks: `w01` → Week 1, `w02` → Week 2, and so on.

### Step 3 — The gate

When the phase's criteria are green, open a PR in your own repo (one per phase) and raise a gate-review issue using the template already in `.github/`. The gate is a viva, not a checklist — you defend the work out loud. Pass, and you move on.

### Step 4 — Phases 2–5

Same move, second starter: **Use this template** on **[elective-craft-track-api](https://github.com/ai-builder-circle/freelanceforge-api-starter)** → your own `freelanceforge-api` → clone it. The loop now runs on concept docs `w04`–`w16`, building module by module per each `BUILD_ORDER.md`, through Gates 2 to 5.

### Step 5 — Finish

Phase 5 is the proof: rebuild one slice in Python from your notes, rewrite your repo's README as a technical essay, record a walkthrough. At that point the two repos in your account *are* the portfolio — the proof of work.

---

## Which repo does what

| Repo | What you do with it |
|------|---------------------|
| `elective-craft-track` (this hub) | Read. Optionally clone to open the concept labs locally. Never build in it. |
| [`elective-craft-track-http`](https://github.com/ai-builder-circle/elective-craft-track-http) | **Use as template** → your own `http-by-hand`. Phase 1. |
| [`elective-craft-track-api`](https://github.com/ai-builder-circle/elective-craft-track-api) | **Use as template** → your own `freelanceforge-api`. Phases 2–5. |
| `you/http-by-hand`, `you/freelanceforge-api` | **Clone these** and build. Your work lives here. |

---

## Opening the concept labs

The concept docs are interactive HTML — GitHub shows them as raw source, not a rendered page. Two ways to actually use them:

**Clone and open locally:**

```bash
git clone git@github.com:ai-builder-circle/elective-craft-track.git
cd elective-craft-track
start concepts/w01-the-wire.html     # Windows.  macOS: open   Linux: xdg-open
```

**Or open the hosted version** (if GitHub Pages is enabled on this repo): the docs become clickable links straight from the table below.

---

## The contract

- You write 100% of the code under `src/`. No AI, no paste-without-understanding.
- Every commit compiles and passes tests.
- `LEARNING.md` updated every working session — the "Unsure" line matters most.
- Come to review with a position, not a question.

---

## Phases

| Phase | Weeks | Concept docs | Gate |
|-------|-------|--------------|------|
| 0 Diagnostic | — | — | send prose answers |
| 1 HTTP & the wire | 1–3 | w01, w02, w03 | concurrent-write walkthrough |
| 2 Domain model | 4–7 | w04–w07 | defend aggregate boundaries |
| 3 Persistence & app layer | 8–10 | w08–w10 | swap the DB in < 1 hour |
| 4 Security | 11–14 | w11–w14 | survive red-team pass |
| 5 Prove it | 15–16 | w15, w16 | rebuild a slice in Python |

---

## Concept documents

Interactive. Each has three layers: a **Lab** you manipulate before reading, a **Chapter** explaining the mechanism, and a **Proof** self-test.

| # | File | Concept |
|---|------|---------|
| 01 | concepts/w01-the-wire.html | HTTP anatomy, framing, statelessness |
| 02 | concepts/w02-representations.html | negotiation, status codes, problem details |
| 03 | concepts/w03-idempotency.html | safety, idempotency, conditional requests, caching |
| 04–16 | *(published as authored)* | domain model → security → transfer |

---

## The whole plan

See [`CURRICULUM.md`](CURRICULUM.md) for the full 16-week programme, phase gates, and primary sources.
