## TDDV: Test‑Driven Development + Validation Starter

A forkable starter to adopt a practical, agent‑assisted TDD workflow. It helps teams ship smaller, safer changes with clear gates and auditable artifacts. See [ABOUT.md](ABOUT.md) for the full methodology.

### Audience

- Developers and development managers adopting or scaling TDD with AI assistants

### Why this workflow

- **Quality and speed**: Tests encode acceptance criteria; short Red → Green → Refactor cycles accelerate delivery without surprises.
- **Smaller, safer PRs**: Focused diffs, easier reviews, and simpler rollbacks.
- **Objective gates**: AC‑mapped tests, CI, and a written Definition of Done decide progress.
- **Team alignment**: Role specialization (QA/Dev/Arch/Review) with clear handoffs and artifacts.
- **Lower onboarding cost**: Durable docs and repeatable prompts reduce ramp‑up time.
- **Works with your tools**: No true multi‑agent required. Use a single IDE agent and, if desired, an external orchestrator to spawn specialized subagents sequentially.

### How it works (at a glance)

- A coordinator orchestrates phases and context; specialized agents execute the steps:
  - QA creates failing tests from acceptance criteria (Red)
  - Developer makes tests pass with minimal change (Green)
  - Architect refactors while keeping tests green (Refactor)
  - Code Reviewer enforces standards and risks (Review)
  - Validate via CI and Definition of Done before merge (Validation)

### Setup & Usage

Placeholder — a setup script and detailed instructions are coming soon.

In the meantime, see `ABOUT.md` for detailed roles, prompts, and tool options.

### Documentation

- Start here for details: [ABOUT.md](ABOUT.md)
- Diagram: `context_pyramid.svg`
- License: `LICENSE`

### References

See the References section in `ABOUT.md` for sources and related tools.


