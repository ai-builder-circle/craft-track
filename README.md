# Craft Track

Build a production-grade REST API by hand — CRUD, security, and
domain-driven design — with **no AI-generated code**. This repo is the
curriculum and the learning materials. You build in your own repo.

> Maintained by FORGE / `ai-builder-circle`. Reference only — nothing to run here.

## How it works

1. Read the concept doc for the week (open the files in `concepts/`).
2. Go to the matching **starter template** and click **Use this template**
   to get your own repo:
   - Phase 1 → https://github.com/ai-builder-circle/http-by-hand-starter
   - Phases 2–5 → https://github.com/ai-builder-circle/freelanceforge-api-starter
3. Follow the `BUILD_ORDER.md` in each package. Write every line yourself.
4. Journal every session in your repo's `LEARNING.md`.
5. Open a PR against your own repo per phase; tag a mentor for review.

## The contract

- You write 100% of the code under `src/`. No AI, no paste-without-understanding.
- Every commit compiles and passes tests.
- `LEARNING.md` updated every working session — the "Unsure" line matters most.
- Come to review with a position, not a question.

## Phases

| Phase | Weeks | Concept docs | Gate |
|-------|-------|--------------|------|
| 0 Diagnostic | — | — | send prose answers |
| 1 HTTP & the wire | 1–3 | w01, w02, w03 | concurrent-write walkthrough |
| 2 Domain model | 4–7 | w04–w07 | defend aggregate boundaries |
| 3 Persistence & app layer | 8–10 | w08–w10 | swap the DB in < 1 hour |
| 4 Security | 11–14 | w11–w14 | survive red-team pass |
| 5 Prove it | 15–16 | w15, w16 | rebuild a slice in Python |

## Concept documents

Interactive. Each has three layers: a **Lab** you manipulate before reading,
a **Chapter** explaining the mechanism, and a **Proof** self-test. Open the
HTML directly in a browser.

| # | File | Concept |
|---|------|---------|
| 01 | concepts/w01-the-wire.html | HTTP anatomy, framing, statelessness |
| 02 | concepts/w02-representations.html | negotiation, status codes, problem details |
| 03 | concepts/w03-idempotency.html | safety, idempotency, conditional requests, caching |
| 04–16 | *(published as authored)* | domain model → security → transfer |

## The whole plan

See [`CURRICULUM.md`](CURRICULUM.md) for the full 16-week programme,
phase gates, and primary sources.
