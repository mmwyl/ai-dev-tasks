# AI Dev Tasks User Guide (English)

This guide explains how to use the AI Dev Tasks rule files (PRD creation, task generation, task processing) with practical, step-by-step instructions for both existing and greenfield projects.

## Who is this for?
- Teams or individuals who want a structured, verifiable workflow for AI-assisted development
- Task breakdowns that are executable by a junior developer

## Quick Start

### Scenario A: Integrate with an Existing Project
1) Place rule files in your repo (root or /ai-dev-tasks):
   - create-prd.md / create-prd.zh-CN.md
   - generate-tasks.md / generate-tasks.zh-CN.md
   - process-task-list.md / process-task-list.zh-CN.md
2) Confirm project standards and commands (reuse them consistently):
   - Test commands (e.g., npm test / pytest / mvn test)
   - Build commands (e.g., npm run build / gradle build)
   - Quality gates (lint/format/static analysis/type-check)
3) Generate tasks from PRD using generate-tasks:
   - Perform Critical Details Extraction and assess current state
   - Identify reusable modules and files; follow established architecture
   - List “Relevant Files” including test files
4) Execute tasks using process-task-list, follow Completion Protocol and perform pre-commit Quality Check.

Recommended improvements (existing projects):
- Reuse first: enforce listing reusable components/dependencies in the “current state assessment”
- Compatibility: check third-party dependencies and language/runtime baselines
- Data migration and rollback: plan migrations/backfills/rollbacks as tasks with acceptance criteria
- Security alignment: align with existing auth/authz; avoid new bypass paths
- Performance budget: quantify targets (e.g., P95/QPS) and include them in acceptance criteria

### Scenario B: Start a New Project (Greenfield)
1) Initialize the project baseline:
   - Create repository and base directory structure
   - Choose package/dependency management
   - Configure testing, lint, format, type-check, pre-commit hooks
   - Define standard scripts: test/build/lint/format/type-check
   - Optional: initialize CI (tests/build/quality gates)
2) Write PRD first using create-prd with Detail Capture Hints
3) Generate tasks using generate-tasks; include 0.x bootstrap tasks:
   - 0.1 Set up tests and quality tools
   - 0.2 Define scripts and base directories
   - 0.3 Configure CI and basic monitoring (optional)
4) Execute with process-task-list; every sub-task must include Acceptance Criteria and Failing Cases.

Recommended improvements (greenfield):
- Standards-first: lock coding standards, directories, and quality gates early; codify in tasks
- Template-driven: PRD and task templates with Detail Checklist and acceptance/failing examples
- Foundational capabilities first: logging, config, error handling, observability
- Incremental delivery: small, verifiable sub-tasks to reduce integration risk

## How-To (Common)

1) Create PRD (create-prd)
- Ask clarifying questions first
- Use Detail Capture Hints to fill edge cases/integrations/data flow/performance/security/UX/analytics/ops

2) Generate Tasks (generate-tasks)
- Perform Critical Details Extraction and confirm alignment with the user
- Phase 1: only parent tasks, pause and wait for “Go”
- Phase 2: sub-tasks must include:
  - Acceptance Criteria (verifiable outcomes)
  - Failing Cases (representative failures and expected handling)
  - Explicit coverage of the six Detail Checklist areas

3) Process Tasks (process-task-list)
- Work on one sub-task at a time; pause after completion
- When a parent task completes: run full tests + Quality Check (Detail Checklist)
- Use Conventional Commits; explain relation to tasks/PRD in messages

## Templates and Checklists

Detail Checklist (apply to parent and sub-tasks as appropriate):
- [ ] Edge cases and error flows
- [ ] External integrations with connection points and contracts
- [ ] Data validation/transformation/persistence
- [ ] Performance (caching/batching/pagination, etc.)
- [ ] Security (authn/authz/input validation/sanitization)
- [ ] UX (loading/empty/error states, accessibility)

Acceptance & Failing examples:
- Form save:
  - Acceptance: valid data persists; field errors; correct loading/disabled state
  - Failing: backend 4xx/5xx; network timeout/retry; duplicate submit; XSS/SQL injection
- List pagination:
  - Acceptance: correct paging/sort/filter; empty/error states; performance target (e.g., P95 < 200ms)
  - Failing: out-of-range page; huge page size; backend latency/failures; stale cache
- Authorization:
  - Acceptance: unauthenticated blocked; role matrix enforced; audit logs present
  - Failing: expired/forged token; bypass paths; multi-role boundaries

## Troubleshooting
- AI not pausing for confirmation: reiterate flow in the task file and use stepwise dialogue
- Missing test commands: add a sub-task to detect and configure project commands
- External credentials: use sandbox/local env vars; never commit secrets
- Failing quality gates: add sub-tasks to fix lint/format/type-check/tests

## Best Practices
- Small commits: minimum verifiable increment per sub-task
- Details-first: align Critical Details before breakdown
- Human-in-the-loop: confirm both plan and details with the user
- Feature flags: gate risky changes with flags

## Appendix: Rule Files
- English: create-prd.md / generate-tasks.md / process-task-list.md
- Chinese: create-prd.zh-CN.md / generate-tasks.zh-CN.md / process-task-list.zh-CN.md