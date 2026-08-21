# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repository is

This is **not a normal codebase** — it is a phased TypeScript learning journey for an early-career developer coming from JavaScript, which gradually grows into a reusable starter template. The authoritative document is **ROADMAP.md**: nine milestone-gated phases (0–8) going from environment setup → TS fundamentals → build tooling → TDD with Vitest → starter template → React → Fastify/Express + Postgres → Docker → GCP Cloud Run deployment tested with Bruno and Playwright.

There are currently no build, test, or lint commands — no code exists yet. As phases introduce tooling (npm scripts in Phase 2, Vitest in Phase 3, docker compose in Phase 6/7), **update this file with the actual commands**.

## How to operate here

- **Just-in-time only.** Never scaffold ahead of the current phase or introduce a tool/library before the phase that needs it (e.g. no Docker before Phase 6, no React before Phase 5). The user explicitly rejected up-front scaffolding as confusing. Check the progress tracker table in ROADMAP.md to see which phase is active.
- **The user is learning, not delegating.** Explain concepts at a beginner-friendly level, prefer guiding over doing, and when writing code, keep it small enough to be understood line by line. In Phase 3 onward, follow strict red–green–refactor: failing test first, commit at red and at green so the cycle is visible in history.
- **Milestones are the gate.** A phase is done when its "you can demonstrate" milestone in ROADMAP.md is observably true — then tick the phase's checkboxes, update the tracker table status, tag the commit (`phase-N-done`), and prompt the user to write a short "what I learned" note in `notes/`.
- **Template quality matters.** From Phase 4 this repo doubles as an onboarding template for less-experienced devs: the README must let a fresh clone reach green tests in under 15 minutes, so keep README setup instructions in sync with any tooling change.

## Conventions

- One growing repo — phases build on each other rather than living in separate directories or repos.
- Git remote: `git@github.com:hemantksingh/ts-starter.git`, branch `main`.
