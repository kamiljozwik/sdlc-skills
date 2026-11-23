## 1. North Star & Agreed Principles

These are the core decisions we have aligned on:

- **Non-Linear SDLC Coverage:** Software development is not a straight line. Every skill must work **standalone** regardless of whether you are doing a 5-minute bug fix, a legacy refactor, a database redesign, or a full greenfield feature.
- **Decoupled by Design:** No skill may force you to run another skill first. Context flows through lightweight artifacts (existing specs, ADRs, git diffs, codebase state) with graceful fallback when those artifacts don't exist.
- **Small, Maintainable Skills:** Skills stay short, readable, and easy to review or evolve. They encode _process and judgment_, not bloated 500-line rulebooks.
- **Context-Aware Engineering (No Blind Dogma):** Never hardcode one philosophy (e.g., always _Clean Code_ or always _Deep Modules_). The agent should choose the right approach for the specific problem while respecting the repository's existing patterns.
- **Strict Precedence Order:** **Repository coding standards and local patterns** always override **generic skill defaults**.
- **Repo-Local & Harness-Portable:** Skills live inside the project repository (not globally on one machine) so cloning into another machine, cloud environment, or sandbox works immediately across popular harnesses (Claude Code, Codex, Antigravity, etc.). This aligns directly with the open **Agent Skills standard (`SKILL.md`)**, where each skill folder holds a concise `SKILL.md` plus on-demand `references/` and `scripts/` directories.

---

## 2. Ground-Truth Knowledge Canon (Researched)

We curated a battle-tested canon combining evergreen classics, modern editions, and living standards across the full SDLC:

| SDLC Domain                   | Core Books & Living Standards                                                                                                                                                                                                                                                        |
| ----------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Discovery & Specs**         | _Continuous Discovery Habits_ (Torres), _Inspired_ (Cagan), _User Story Mapping_ (Patton), _Specification by Example_ (Adzic), _Software Requirements_ (Wiegers)                                                                                                                     |
| **Architecture & ADRs**       | _Fundamentals of Software Architecture_ & _The Hard Parts_ (Richards/Ford), _Learning DDD_ (Khononov), _DDD_ (Evans), _Building Microservices_ (Newman), **C4 Model**, **MADR/Nygard ADRs**                                                                                          |
| **Code Craft & Refactoring**  | _A Philosophy of Software Design_ (Ousterhout), _Balancing Coupling_ (Khononov), _Tidy First?_ (Beck), _Refactoring_ (Fowler), _Working Effectively with Legacy Code_ (Feathers), _The Pragmatic Programmer_, _Clean Code_, _Code Complete_, _Your Code as a Crime Scene_ (Tornhill) |
| **Data & Databases**          | _Designing Data-Intensive Applications 2nd Ed._ (Kleppmann/Riccomini), _SQL Antipatterns_ (Karwin), _Database Design for Mere Mortals_ (Hernandez), _The Data Warehouse Toolkit_ (Kimball), _Database Internals_ (Petrov)                                                            |
| **Testing & QA**              | _Unit Testing: Principles, Practices, and Patterns_ (Khorikov), _TDD By Example_ (Beck), _GOOS_ (Freeman/Pryce), _xUnit Test Patterns_, _Agile Testing_                                                                                                                              |
| **Code Reviews & Standards**  | _"Looks Good to Me"_ (Braganza), _Software Engineering at Google_, **Google Engineering Practices**                                                                                                                                                                                  |
| **Deployment, DevOps & SRE**  | _Continuous Delivery_ (Humble/Farley), _Modern Software Engineering_ (Farley), _DevOps Handbook_, _Google SRE_, _Release It!_ (Nygard), **OWASP ASVS / Testing Guide**                                                                                                               |
| **Process, Metrics & Retros** | _Agile Retrospectives 2nd Ed._ (Derby/Larsen/Horowitz), _Accelerate_ (Forsgren et al.), _Team Topologies_, _The Mythical Man-Month_, **DORA Research**                                                                                                                               |

---

## 3. Target Architecture: Three Layers + Two-Speed Knowledge

We moved away from plugging a live raw-book RAG directly into day-to-day coding (due to context dilution, poor chunking of narrative books, inconsistent style drift across sessions, and broken repo portability) in favor of a cleaner **three-layer repo architecture**:

1. **Repo Context Layer (Per-Project Truth):** Files like `CODING_STANDARDS.md`, `GLOSSARY.md`, `AGENTS.md`/`CLAUDE.md`, and `docs/adr/` that define how _this specific repo_ works.

- _Open Concept:_ A **Standards Bootstrapper** skill that inspects a repo's stack, architecture, and problem domain upfront and drafts tailored coding standards grounded in our canon.

2. **Minimal Skills Layer (Workflows vs. Disciplines):**

- **User-Invoked Workflows:** Explicit entry points when you enter a mode of work (Discover, Specify, Architect, Model Data, Implement, Refactor, Review, Ship/Operate, Reflect).
- **Model-Invoked Disciplines:** Reusable craft skills the agent pulls in automatically when relevant (e.g., Vertical TDD, Bug Diagnosis, Security Pass, Session Handoff).

3. **Two-Speed Knowledge Layer:**

- **Execution Speed (Inside the Repo):** Compact 1-page **Distilled Trade-Off Cards** stored in `references/` next to skills (e.g., `references/modularity.md`, `references/sql-design.md`). Instead of hardcoding one dogma, each card defines _when_ to apply competing principles (e.g., Ousterhout vs. Beck/Clean Code) so the agent makes grounded, context-aware choices with zero external infrastructure.
- **Research Speed (Optional / Offline RAG):** A personal RAG system over your full book library used _offline_ to ingest new books and update your distilled cards—or optionally called as a "Librarian" tool only during heavy, high-stakes Architecture/Discovery research.

---

## 4. How We Compare with Latest `mattpocock/skills`

Studying the current `mattpocock/skills` pack (~24 skills spanning `/setup-matt-pocock-skills`, `/grill-with-docs`, `/to-spec`, `/to-tickets`, `/triage`, `/implement`, `/wayfinder`, `/improve-codebase-architecture`, `/code-review`, `/retro`, plus model-invoked disciplines like `tdd`, `diagnosing-bugs`, `codebase-design`, `research`, and `writing-for-agents`) clarifies what to borrow and where we diverge:

| Dimension               | Latest `mattpocock/skills`                                                                                                                                                 | Our Planned Approach                                                                                                                                          |
| ----------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Core Split**          | User-invoked workflows + Model-invoked disciplines (user skills never call other user skills).                                                                             | **Adopt.** Keeps slash commands small while reusing core craft disciplines underneath.                                                                        |
| **Context Discovery**   | `/setup-matt-pocock-skills` writes `docs/agents/` config files; `/code-review` uses graceful fallback lookup and parallel two-axis review (standards + spec faithfulness). | **Adopt & Simplify.** Keep graceful fallback lookup and multi-axis review, but avoid requiring an upfront issue-tracker setup wizard.                         |
| **Workflow Gravity**    | Heavily anchored around an issue tracker (GitHub, GitLab, or local markdown tickets) and a `triage` label state machine.                                                   | **Diverge.** Artifact-first and tracker-optional so you can drop into any SDLC phase with zero ticket ceremony.                                               |
| **SDLC Coverage**       | Strong on grilling, specs, tickets, TDD, deep modules, and PR review; light on DB modeling, deployment/SRE, security/OWASP, and formal architecture trade-offs.            | **Expand.** Cover the full lifecycle, including Data Modeling, Architecture/ADRs, Deployment/Reliability, and Security.                                       |
| **Standards & Retros**  | `/retro` patches `CODING_STANDARDS.md` reactively after session friction.                                                                                                  | **Expand.** Combine proactive Standards Bootstrapping (from repo stack + domain) with both agent-tuning retros and broader engineering/hotspot health checks. |
| **Knowledge Grounding** | Implicitly baked into each skill's prompt (e.g., Ousterhout in `codebase-design`, Fowler smells in `code-review`).                                                         | **Evolve.** Explicit, editable `references/` trade-off cards inside the repo so principles are transparent, non-dogmatic, and easy to update.                 |

---

## 5. What Is Still Blurry / Open for Next Discussions

Before writing any actual `SKILL.md` files, these six areas still need to be pinned down:

1. **Exact Skill Roster & Granularity:** How many total skills hit the sweet spot between full SDLC coverage and low cognitive overhead? (e.g., Should _Debug_, _Deploy/CI-CD_, and _SRE/Reliability_ be separate skills or grouped? Does _Retrospective_ belong inside the coding agent?)
2. **User-Invoked vs. Model-Invoked Boundary:** Which exact capabilities should you trigger manually, and which should the agent auto-invoke? (Over-relying on auto-invocation can be unpredictable across harnesses if descriptions get truncated.)
3. **Interactive vs. Autonomous Modes:** Explicitly tagging which skills must interview you and push back (Discovery, Architecture, Data Modeling) vs. which should execute autonomously with minimal interruption (Refactor, Review, Bug Diagnosis).
4. **Cross-Harness Repo Layout:** Different tools read slightly different project folders—`.agents/skills/` is emerging as the neutral path for Codex/Copilot/OpenCode, while Claude Code reads `.claude/skills/`. We need a clean convention (single source + symlink/sync, or native neutral layout) so there's zero duplication drift.
5. **The Standards Bootstrapper Scope:** Exactly what files it inspects, what it generates (`CODING_STANDARDS.md`, `AGENTS.md` pointer, etc.), and how to keep generated standards short enough not to waste context tokens on every turn.
6. **Trade-Off Card Format & MVP Slice:** Designing the exact template for a 1-page `references/*.md` card and picking **2–3 high-value starter skills** to prototype and battle-test in a real repo before building the rest.
