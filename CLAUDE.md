# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repository is

This is **not a normal codebase** — it is a phased TypeScript learning journey for an early-career developer coming from JavaScript, which gradually grows into a reusable starter template. The authoritative document is **ROADMAP.md**: nine milestone-gated phases (0–8) going from environment setup → TS fundamentals → build tooling → TDD with Vitest → starter template → React → Fastify/Express + Postgres → Docker → GCP Cloud Run deployment tested with Bruno and Playwright.

From Phase 1 onward every phase builds a piece of **one product — an expense splitter** (Splitwise-style: shared expenses, uneven splits, penny-exact rounding, who-pays-whom settlement) — so domain code written early survives into the deployed system. Phase 3 alone warms up on a throwaway kata before TDD-ing the real rules. The schedule is deliberately aggressive (**6 weeks at ~12–15 hrs/week**) on the assumption the learner uses Claude as a tutor to unblock fast — not as a code generator.

There are currently no build, test, or lint commands — no code exists yet. As phases introduce tooling (npm scripts in Phase 2, Vitest in Phase 3, docker compose in Phase 6/7), **update this file with the actual commands**.

## How to operate here

- **Just-in-time only.** Never scaffold ahead of the current phase or introduce a tool/library before the phase that needs it (e.g. no Docker before Phase 6, no React before Phase 5). The user explicitly rejected up-front scaffolding as confusing. Check the progress tracker table in ROADMAP.md to see which phase is active.
- **The user is learning, not delegating.** Explain concepts at a beginner-friendly level, prefer guiding over doing, and when writing code, keep it small enough to be understood line by line. In Phase 3 onward, follow strict red–green–refactor: failing test first, commit at red and at green so the cycle is visible in history.
- **Milestones are the gate.** A phase is done when its "you can demonstrate" milestone in ROADMAP.md is observably true — then tick the phase's checkboxes, update the tracker table status, tag the commit (`phase-N-done`), and prompt the user to write a short "what I learned" note in `notes/`.
- **Template quality matters.** From Phase 4 this repo doubles as an onboarding template for less-experienced devs: the README must let a fresh clone reach green tests in under 15 minutes, so keep README setup instructions in sync with any tooling change.

## Conventions

- One growing repo — phases build on each other rather than living in separate directories or repos.
- Git remote: `git@github.com:hemantksingh/ts-starter.git`, branch `main`.
