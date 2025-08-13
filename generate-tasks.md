# Rule: Generating a Task List from a PRD

## Goal

To guide an AI assistant in creating a detailed, step-by-step task list in Markdown format based on an existing Product Requirements Document (PRD). The task list should guide a developer through implementation.

## Output

- **Format:** Markdown (`.md`)
- **Location:** `/tasks/`
- **Filename:** `tasks-[prd-file-name].md` (e.g., `tasks-prd-user-profile-editing.md`)

## Process

1.  **Receive PRD Reference:** The user points the AI to a specific PRD file
2.  **Analyze PRD:** The AI reads and analyzes the functional requirements, user stories, and other sections of the specified PRD.
3.  **Assess Current State:** Review the existing codebase to understand existing infrastructre, architectural patterns and conventions. Also, identify any existing components or features that already exist and could be relevant to the PRD requirements. Then, identify existing related files, components, and utilities that can be leveraged or need modification. Additionally, detect the repository's primary language(s) and tooling (build system, package manager, test runner, framework) to ensure the plan and examples (e.g., file paths, test commands) are language-agnostic and correctly adapted to this project.
4.  **Phase 1: Generate Parent Tasks:** Based on the PRD analysis and current state assessment, create the file and generate the main, high-level tasks required to implement the feature. Use your judgement on how many high-level tasks to use. It's likely to be about 5. Present these tasks to the user in the specified format (without sub-tasks yet). Inform the user: "I have generated the high-level tasks based on the PRD. Ready to generate the sub-tasks? Respond with 'Go' to proceed."
5.  **Wait for Confirmation:** Pause and wait for the user to respond with "Go".
6.  **Phase 2: Generate Sub-Tasks:** Once the user confirms, break down each parent task into smaller, actionable sub-tasks necessary to complete the parent task. Ensure sub-tasks logically follow from the parent task, cover the implementation details implied by the PRD, and consider existing codebase patterns where relevant without being constrained by them.
7.  **Identify Relevant Files:** Based on the tasks and PRD, identify potential files that will need to be created or modified. List these under the `Relevant Files` section, including corresponding test files if applicable.
8.  **Generate Final Output:** Combine the parent tasks, sub-tasks, relevant files, and notes into the final Markdown structure.
9.  **Save Task List:** Save the generated document in the `/tasks/` directory with the filename `tasks-[prd-file-name].md`, where `[prd-file-name]` matches the base name of the input PRD file (e.g., if the input was `prd-user-profile-editing.md`, the output is `tasks-prd-user-profile-editing.md`).

## Output Format

The generated task list _must_ follow this structure:

```markdown
## Relevant Files

- `path/to/module/file.ext` - Brief description (e.g., core module/class/function for this feature).
- `tests/module/file.test.ext` or `path/to/module/file_spec.ext` - Tests for `file.ext` (use your project's naming convention).
- `path/to/service-or-endpoint.ext` - API/service layer changes if applicable.
- `path/to/ui-component.ext` - UI component or template if applicable.
- `scripts/migration_or_job.ext` - Data migrations, background jobs, or scripts if applicable.

### Notes

- Place tests according to your ecosystem's convention:
  - JavaScript/TypeScript: alongside as `*.test.ts(x)`/`*.spec.ts(x)` or under `__tests__/`
  - Python: under `tests/` as `test_*.py`
  - Java/Kotlin: under `src/test/java|kotlin`
  - Go: in same package as `*_test.go`
  - Ruby: under `spec/` (RSpec) or `test/` (Minitest)
  - Rust: `tests/` integration tests, unit tests inline with `#[cfg(test)]`
  - .NET: in `*.Tests/` projects
- Run your project's test command. Examples by ecosystem:
  - Node.js: `npm test` / `pnpm test` / `yarn test`
  - Python: `pytest`
  - Go: `go test ./...`
  - Java: `mvn test` or `gradle test`
  - Ruby: `bundle exec rspec` or `bin/rails test`
  - Rust: `cargo test`
  - .NET: `dotnet test`

## Tasks

- [ ] 1.0 Parent Task Title
  - [ ] 1.1 [Sub-task description 1.1]
  - [ ] 1.2 [Sub-task description 1.2]
- [ ] 2.0 Parent Task Title
  - [ ] 2.1 [Sub-task description 2.1]
- [ ] 3.0 Parent Task Title (may not require sub-tasks if purely structural or configuration)
```

## Interaction Model

The process explicitly requires a pause after generating parent tasks to get user confirmation ("Go") before proceeding to generate the detailed sub-tasks. This ensures the high-level plan aligns with user expectations before diving into details.

## Target Audience

Assume the primary reader of the task list is a **junior developer** who will implement the feature with awareness of the existing codebase context.