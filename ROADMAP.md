# TypeScript Learning Roadmap

A phased path from "I know JavaScript" to "I can build, test, containerise, and deploy a real TypeScript system." Written for an early-career developer with a JS background (variables, conditionals, loops, functions), and designed to double as an onboarding guide for other junior devs later.

**The problem you'll solve: an expense splitter.** From Phase 1 onward, every phase builds a piece of the same product — a Splitwise-style app where a group records shared expenses and the app works out who owes whom. It's small enough to hold in your head but has real business rules: money handling, uneven splits, rounding pennies fairly, settling balances. That means the types you write in Phase 1, the logic you TDD in Phase 3, the UI from Phase 5 and the API from Phase 6 all survive into the system you deploy in Phase 8 — nothing you build is throwaway. The one deliberate exception: Phase 3 warms up on a small kata first, because when you're learning red–green–refactor you want a problem whose answer you already know, so all your attention goes on the cycle.

**Guiding principle: just-in-time learning.** No tool, library, or concept is introduced before the phase that needs it. Nothing is scaffolded up front. Each phase is gated by the previous phase's milestone — you move on when you can *demonstrate* the milestone, not when you've read about the topic.

**How to use this doc**

- Work through phases in order. Tick the `- [ ]` checkboxes as you go.
- Every phase ends with a **Milestone** phrased as something observable — a passing test run, a working demo, a URL. That's your evidence of progress (useful for a mentor or manager).
- When you hit a milestone, tag it in git (`git tag phase-1-done`) and write a short "what I learned" note (3–5 bullets) in a `notes/` folder. The tags + notes are your progress trail.
- Keep everything in **one growing repo** (this one). Each phase builds on the last, which mirrors how real projects evolve — and one repo with milestone tags demonstrates progress far better than scattered experiments.
- Effort estimates assume an apprentice pace with Claude as an accelerator: **~12–15 focused hours per week**, whole roadmap targeted at **6 weeks**. Week numbers are the schedule; milestones are the gate. If a milestone isn't demonstrable, take the extra days — skipping milestones is not.
- **Use Claude hard — as a tutor, not a typist.** Ask it to explain concepts, review your code, set you practice exercises, and unblock you fast when you're stuck (stuck-time is where most learning schedules die, and removing it is why this one is aggressive). But write the code yourself: code Claude writes for you is learning you didn't do, and the milestones test *you*, not it.

## Progress tracker

| Phase | Topic | Target week | Status | Milestone (proof) |
|---|---|---|---|---|
| 0 | Environment & workflow | Week 1, day 1 | ⬜ Not started | Hello-world TS repo, reproducible from your notes |
| 1 | TypeScript fundamentals (JS → TS) | Week 1 | ⬜ Not started | Typed expense-splitter domain library under `strict: true` |
| 2 | Build system & tooling | Week 2 (first half) | ⬜ Not started | One command builds, lints, and runs the project |
| 3 | Testing & TDD (red-green-refactor) | Weeks 2–3 | ⬜ Not started | Split/settlement rules built strictly RGR, visible in commit history |
| 4 | The starter template | Week 3 | ⬜ Not started | Fresh clone → green tests in under 15 min via README |
| 5 | React with TypeScript | Weeks 3–4 | ⬜ Not started | Expense-splitter UI with passing component tests |
| 6 | Backend, Postgres & first Docker | Weeks 4–5 | ⬜ Not started | API persisting to Postgres-in-Docker, integration tests green |
| 7 | Docker for the dev | Week 5 | ⬜ Not started | `docker compose up` runs the whole stack from clean checkout |
| 8 | Deploy & test on GCP Cloud Run | Week 6 | ⬜ Not started | Public URL + green Bruno collection + green Playwright smoke test |

Statuses: ⬜ Not started · 🟨 In progress · ✅ Done (update the table as you go)

---

## Phase 0 — Environment & workflow

**Goal:** a working, repeatable local dev setup — the boring stuff that blocks everyone's first day.

**Effort:** ~half a day (mostly revision if you've been through a code academy) — Week 1, day 1.

**What you'll learn**

- [ ] Install Node via a version manager (`nvm` or `fnm`) and understand why versions are pinned (`.nvmrc`)
- [ ] Initialise a git repo; commit early and often with meaningful messages
- [ ] `npm init`, what `package.json` is, dependencies vs devDependencies
- [ ] Install TypeScript locally (`npm i -D typescript`) and run `npx tsc --version`
- [ ] Editor setup: VS Code (or similar) with TypeScript language support — hover types, go-to-definition, inline errors

**Practice / deliverable:** this repo, initialised, with a `src/index.ts` that prints something, run via `npx tsc && node dist/index.js`.

**Milestone — you can demonstrate:** starting from a machine with nothing installed, your own written setup notes get you (or someone else) to a running hello-world TypeScript repo. Those notes become the seed of the Phase 4 README.

---

## Phase 1 — TypeScript fundamentals (JS → TS)

**Goal:** translate the JavaScript you already know into typed TypeScript, and understand what the compiler buys you.

**Effort:** ~12–15 hrs — Week 1. This is the foundation: the one phase not to shortcut if generics or narrowing still feel shaky at the end.

**What you'll learn**

- [ ] Type annotations on what you already know: variables, function parameters and return types
- [ ] Primitive types (`string`, `number`, `boolean`), arrays, objects
- [ ] Type inference — when you *don't* need to annotate
- [ ] `interface` and `type` aliases for object shapes
- [ ] Union types (`string | null`) and narrowing with `typeof` / `in` / equality checks
- [ ] Literal types and `as const`
- [ ] Optional properties (`?`) and `undefined` vs `null`
- [ ] Functions as types; typing callbacks
- [ ] Generics — just the intro: `Array<T>`, writing one simple generic function
- [ ] `tsconfig.json`: what `strict: true` turns on, and why you always want it
- [ ] The core mental model: **types exist at compile time only** — compile errors vs runtime errors
- [ ] `any` vs `unknown`, and why `any` is a last resort

**Practice / deliverable:** the expense splitter's domain library in `src/domain/` — `Money` as integer pence (never floats: try `0.1 + 0.2` in a JS console and see why), `Person`, `Expense` with its split, and a `Result`-style success/failure type for validating input (e.g. rejecting a negative amount). Plus simple pure functions over them: `addMoney`, `validateExpense`. Break things on purpose and read the compiler errors until they make sense.

**Milestone — you can demonstrate:** your domain library compiles cleanly under `strict: true`, and you can walk someone through two or three real mistakes the compiler caught for you (mixing pence with pounds, a forgotten `undefined` check) that plain JS would have let through to runtime.

---

## Phase 2 — Build system & tooling

**Goal:** demystify what happens between the `.ts` you write and the code that runs.

**Effort:** ~5–6 hrs — Week 2, first half. Mostly revision for a code-academy grad; ESM vs CommonJS is the genuinely new part.

**What you'll learn**

- [ ] What `tsc` actually does: type-checks *and* emits JS; source (`src/`) → output (`dist/`)
- [ ] Key `tsconfig.json` options: `target`, `module`, `outDir`, `rootDir`, `strict`
- [ ] ESM vs CommonJS — what `"type": "module"` means and why import errors happen
- [ ] Running TS directly in dev with `tsx` vs building with `tsc` for production — and when each is right
- [ ] npm scripts as the project's command palette: `dev`, `build`, `start`, `lint`, `typecheck`
- [ ] ESLint (with typescript-eslint) for catching problems; Prettier for formatting; why they're separate jobs
- [ ] Editor integration: format-on-save, lint squiggles

**Practice / deliverable:** wire this repo up so `npm run dev` runs instantly via tsx, `npm run build` emits `dist/`, `npm run lint` and `npm run typecheck` gate quality.

**Milestone — you can demonstrate:** a single command (`npm run check` or similar chaining lint + typecheck + build) passes cleanly, and you can explain the journey from `src/index.ts` to `dist/index.js` to a running Node process without hand-waving.

---

## Phase 3 — Testing & TDD (red-green-refactor)

**Goal:** make tests your default way of writing code, not an afterthought.

**Effort:** ~10–12 hrs of **daily deliberate practice** — Weeks 2–3. This phase compresses least, Claude or no Claude: RGR is habit formation, not information, so short daily sessions beat one long one.

**What you'll learn**

- [ ] Vitest: install, `describe` / `it` / `expect`, `npm test`
- [ ] Watch mode — the RGR feedback loop lives here
- [ ] The cycle: **Red** (write a failing test first) → **Green** (simplest code that passes) → **Refactor** (clean up with the tests as a safety net)
- [ ] Arrange–Act–Assert structure; one behaviour per test; naming tests by behaviour
- [ ] Testing edge cases: empty inputs, boundaries, error paths
- [ ] Test doubles (mocks/stubs) — awareness only; you'll use them properly in Phase 6
- [ ] Retrofitting: add tests to your Phase 1 domain library, then refactor it with confidence

**Practice / deliverable:** warm up with **one kata session** (FizzBuzz or the String Calculator), strictly test-first — a throwaway problem you already know the answer to, so all your attention goes on the cycle. Then TDD the expense splitter's real rules: splitting an expense unevenly, rounding so the pennies still add up to the total (£10 three ways is the classic), computing each person's balance, and working out who pays whom to settle. Commit at every red and every green so the cycle is visible in history.

**Milestone — you can demonstrate:** the split and settlement rules built with a commit history that reads red → green → refactor → red → green…, the whole suite green in one `npm test` run, and the rounding edge case covered by a test. Bonus proof: refactor something aggressively and show the tests catching a mistake.

---

## Phase 4 — The starter template (consolidation)

**Goal:** turn Phases 0–3 into the reusable `ts-starter` template — the original purpose of this repo, earned rather than copied.

**Effort:** ~4–5 hrs — Week 3.

**What you'll learn**

- [ ] Writing a README that a less-experienced dev can actually follow: prerequisites, setup, every npm script explained, project layout
- [ ] What belongs in a template vs what's project-specific
- [ ] `.gitignore`, `.nvmrc`, editor config — the small files that make onboarding smooth
- [ ] (Optional) a pre-commit hook running lint + tests

**Practice / deliverable:** the polished template: `src/`, `tests/` (or co-located tests), tsconfig, ESLint/Prettier config, Vitest config, npm scripts, README. Your Phase 1–3 domain module with its tests doubles as the example showing the TDD style you expect — and is a live case study of "what belongs in a template vs what's project-specific" (config: template; expense-splitting rules: project).

**Milestone — you can demonstrate:** someone else (or you, in a fresh directory) clones the repo and gets from zero to green tests in **under 15 minutes** using only the README. If they get stuck, the README — not the person — gets fixed.

---

## Phase 5 — React with TypeScript

**Goal:** apply your TS foundation to building user interfaces.

**Effort:** ~15–18 hrs — Weeks 3–4. With prior React exposure from a code academy this goes faster; the genuinely new material is typing components and testing with React Testing Library.

**What you'll learn**

- [ ] Scaffold with Vite (`npm create vite@latest -- --template react-ts`) and recognise your Phase 2 knowledge in its config
- [ ] Components and JSX/TSX; typing props with interfaces
- [ ] `useState` and `useEffect`, with types (mostly inferred)
- [ ] Handling events and forms with correct event types
- [ ] Lifting state up; passing typed callbacks down
- [ ] Conditional rendering and rendering lists (keys)
- [ ] Component testing with Vitest + React Testing Library — test what the user sees, TDD where it fits
- [ ] Fetching data from an API and typing the response (this sets up Phase 6)

**Practice / deliverable:** the expense splitter's UI: add people to a group, record an expense with its split, and watch everyone's balance and the "who pays whom" settlement update — all powered by the domain library you TDD'd in Phase 3, imported straight into the frontend. Keep state in memory for now; persistence is deliberately deferred to Phase 6.

**Milestone — you can demonstrate:** the app running locally (`npm run dev`), splitting real expenses interactively, with component tests passing — and you can explain how TypeScript caught prop/state mistakes while you built it.

---

## Phase 6 — Backend, Postgres & first Docker exposure

**Goal:** give your app a real backend with real persistence — and meet Docker at exactly the moment it becomes useful.

**Effort:** ~15–20 hrs — Weeks 4–5. The biggest phase; split it into API-first, then DB.

> **When to introduce Docker — answered.** You asked at what stage Docker should enter the learning path. The answer this roadmap takes: **here, as a consumer first.** You need a Postgres database; installing one natively is fiddly and pollutes your machine; `docker compose up` gives you a disposable, identical database in one command. That's the dev-POV value of Docker in a nutshell — experience it as a user before authoring images yourself (that's Phase 7). No Docker earlier than this, because nothing earlier needed it.

**What you'll learn**

- [ ] A minimal HTTP API with Fastify or Express: routes, request/response, JSON, status codes
- [ ] Typing request bodies and responses; validating input at the boundary (e.g. with zod) — where compile-time types meet runtime data
- [ ] Docker as a consumer: images vs containers, `docker compose up/down`, ports, volumes (so your data survives restarts), just enough to run Postgres
- [ ] SQL basics against your containerised Postgres: `CREATE TABLE`, `SELECT`, `INSERT`, `UPDATE`, `DELETE`, a join
- [ ] Talking to Postgres from TS (the `pg` client, or a light query builder/ORM like Drizzle or Kysely — pick one, don't tour them all)
- [ ] Environment variables and config (`DATABASE_URL`, dotenv locally)
- [ ] Integration tests with Vitest that hit the real containerised database — and why they're different from unit tests
- [ ] Connect the Phase 5 React app to this API

**Practice / deliverable:** the expense splitter's API behind your Phase 5 app (people, expenses and splits persisted to Postgres; balances computed by your Phase 3 domain logic), with Postgres supplied by a `docker-compose.yml` you copy-and-understand, and an integration test suite.

**Milestone — you can demonstrate:** stop the API, restart it, and the data is still there (it lives in Postgres, running in Docker); the integration tests pass against the containerised DB; the React app reads and writes through the API end-to-end on your machine.

---

## Phase 7 — Docker for the dev (authoring)

**Goal:** move from consuming containers to authoring them — dev-level competence, deliberately not expertise.

**Effort:** ~5–6 hrs — Week 5 (you've already been consuming Docker since Phase 6).

**What you'll learn**

- [ ] The mental model: image = frozen recipe + filesystem; container = a running instance of it
- [ ] Write a `Dockerfile` for your API: base image, `COPY`, `RUN npm ci`, build, `CMD`
- [ ] Layers and layer caching — why the order of Dockerfile lines matters for rebuild speed
- [ ] Multi-stage builds: build with dev deps, ship a slim runtime image
- [ ] `.dockerignore` (the `node_modules` lesson everyone learns once)
- [ ] Extend `docker-compose.yml` to run **app + database together**, with networking between them and env vars for config
- [ ] Reading container logs and `exec`-ing into a container to debug
- [ ] Explicitly out of scope: Kubernetes, orchestration, image hardening — know they exist, move on

**Practice / deliverable:** your API containerised; one `docker-compose.yml` that brings up the full stack.

**Milestone — you can demonstrate:** from a clean checkout on any machine with Docker, `docker compose up` starts the API and Postgres together and the app works — no "runs on my machine" caveats. This is also the direct enabler for Phase 8: Cloud Run runs exactly the image you just learned to build.

---

## Phase 8 — Deploy & test on GCP Cloud Run

**Goal:** ship it — your container running on real cloud infrastructure, verified by real tests.

**Effort:** ~8–10 hrs — Week 6. First-time GCP friction (billing, IAM, Cloud SQL connectivity) is the schedule risk here; the GitHub Actions item is stretch/overflow, not part of the 6 weeks.

**What you'll learn**

- [ ] GCP basics: a project, the `gcloud` CLI, authentication
- [ ] Push your image to Artifact Registry
- [ ] Deploy to Cloud Run: why your Phase 7 Dockerfile is exactly what it wants; the `PORT` env var contract; request-based autoscaling (including to zero)
- [ ] Configuration and secrets in the cloud: env vars on the service, Secret Manager for the DB password
- [ ] A database for the deployed app: Cloud SQL for Postgres (managed) — and why you don't run your DB *in* Cloud Run
- [ ] Watching logs and debugging a deployed service (Cloud Logging)
- [ ] **Bruno**: build a collection covering your API's endpoints, with environments for local vs deployed
- [ ] **Playwright**: a smoke E2E test that drives the deployed app like a user (load page → add an expense → see balances update and persist)
- [ ] Cost awareness: free tiers, and tearing down what you're not using
- [ ] (Stretch) automate it: a GitHub Actions workflow that builds, tests, and deploys on push

**Practice / deliverable:** your Phase 6/7 system live on Cloud Run, a committed Bruno collection, and a Playwright smoke suite pointed at the deployed URL.

**Milestone — you can demonstrate:** a public Cloud Run URL where the app works end-to-end; the Bruno collection runs green against it; the Playwright smoke test runs green against it. That's a deployed, tested, containerised TypeScript system — the finish line of this roadmap.

---

## After the roadmap

Directions this foundation supports, in no particular order: authentication and sessions; database migrations as a first-class practice; CI/CD depth; monorepo tooling; performance and observability; a second real project built end-to-end without the training wheels. By then, this doc's job is done — hand the Phase 4 template and this roadmap to the next junior dev.
