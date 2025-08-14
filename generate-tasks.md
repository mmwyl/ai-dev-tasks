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
3.  **Critical Details Extraction:** Before proceeding, explicitly identify and document:
    - **Edge Cases:** What error conditions, boundary cases, or exceptional scenarios are mentioned or implied?
    - **Integration Points:** What external APIs, databases, services, or third-party systems need to be integrated?
    - **Data Flow:** What data transformations, validations, or persistence operations are required?
    - **Performance Requirements:** Any specific performance, scalability, or optimization requirements?
    - **Security Considerations:** Authentication, authorization, data validation, or other security requirements?
    - **User Experience Details:** Loading states, error messages, feedback mechanisms, accessibility requirements?
5.  **Assess Current State:** Review the existing codebase to understand existing infrastructre, architectural patterns and conventions. Also, identify any existing components or features that already exist and could be relevant to the PRD requirements. Then, identify existing related files, components, and utilities that can be leveraged or need modification.
6.  **Phase 1: Generate Parent Tasks:** Based on the PRD analysis and current state assessment, create the file and generate the main, high-level tasks required to implement the feature. Use your judgement on how many high-level tasks to use. It's likely to be about 5. Present these tasks to the user in the specified format (without sub-tasks yet). Inform the user: "I have generated the high-level tasks based on the PRD. Ready to generate the sub-tasks? Respond with 'Go' to proceed."
7.  **Wait for Confirmation:** Pause and wait for the user to respond with "Go".
8.  **Phase 2: Generate Sub-Tasks:** Once the user confirms, break down each parent task into smaller, actionable sub-tasks necessary to complete the parent task. **Critical: Ensure each sub-task explicitly addresses the details identified in step 3.** Sub-tasks must include:
    - Explicit handling of edge cases and error scenarios
    - Integration implementation with specific connection points
    - Data validation, transformation, and persistence steps
    - Performance considerations (caching, optimization, etc.)
    - Security implementation (validation, sanitization, authentication checks)
    - User experience elements (loading states, error handling, feedback)
7.  **Identify Relevant Files:** Based on the tasks and PRD, identify potential files that will need to be created or modified. List these under the `Relevant Files` section, including corresponding test files if applicable.
8.  **Generate Final Output:** Combine the parent tasks, sub-tasks, relevant files, and notes into the final Markdown structure.
9.  **Save Task List:** Save the generated document in the `/tasks/` directory with the filename `tasks-[prd-file-name].md`, where `[prd-file-name]` matches the base name of the input PRD file (e.g., if the input was `prd-user-profile-editing.md`, the output is `tasks-prd-user-profile-editing.md`).

## Output Format

The generated task list _must_ follow this structure:

```markdown
## Execution Configuration

**Execution Mode**: `Default` | `Auto`
- **Default**: Pause after each sub-task for user confirmation
- **Auto**: Execute all sub-tasks within a parent task continuously, pause only after parent task completion
- **Quality Gates**: All tests, quality checks, and commit protocols remain unchanged regardless of mode

## Relevant Files

AI should identify and list files following these patterns:

- **Core Implementation Files**: Primary source files that implement the feature functionality
- **Test Files**: Corresponding test files for each core implementation file
- **Configuration Files**: Build, dependency, or framework configuration files that may need updates
- **Documentation Files**: API docs, README sections, or other documentation requiring updates
- **Dependency Files**: Shared utilities, libraries, or modules that the feature depends on or modifies

**Format**: `relative/path/to/file` - Clear description of the file's role in this feature

### Notes

AI should provide project-specific guidance following these principles:

- **Test Organization**: Describe where tests should be placed according to the project's existing patterns
- **Build Commands**: Identify the commands needed to build, test, and validate changes in this project
- **Quality Gates**: List any linting, formatting, or static analysis tools configured in the project
- **Dependencies**: Note any package installations or dependency updates required
- **Integration Points**: Highlight files or systems that integrate with this feature

## Tasks

- [ ] 1.0 Parent Task Title
  - [ ] 1.1 [Sub-task description 1.1]
        - Acceptance Criteria: [List concrete, verifiable outcomes]
        - Failing Cases: [List representative failure scenarios and expected handling]
  - [ ] 1.2 [Sub-task description 1.2]
        - Acceptance Criteria: [List concrete, verifiable outcomes]
        - Failing Cases: [List representative failure scenarios and expected handling]
- [ ] 2.0 Parent Task Title
  - [ ] 2.1 [Sub-task description 2.1]
- [ ] 3.0 Parent Task Title (may not require sub-tasks if purely structural or configuration)

## Detail Checklist

For each parent task and its sub-tasks, ensure the following are explicitly covered where applicable:
- [ ] Edge cases and error flows are identified and handled
- [ ] External integrations are specified with connection points and contracts
- [ ] Data validation/transformation/persistence steps are present
- [ ] Performance considerations (caching, batching, pagination, etc.) are addressed
- [ ] Security concerns (authn/authz/input validation/sanitization) are addressed
- [ ] UX details (loading, empty state, error messages, accessibility) are included
```

## Interaction Model

The process explicitly requires a pause after generating parent tasks to get user confirmation ("Go") before proceeding to generate the detailed sub-tasks. For execution, the task list may declare an "Execution Mode" in the "Execution Configuration" section (`Default` or `Auto`). In `Auto` mode, the AI executes all sub-tasks under a parent task continuously and pauses only after the parent task completes for review; quality gates remain unchanged. Before proceeding, the AI must review the "Critical Details Extraction" with the user and confirm alignment. This ensures both the high-level plan and the key implementation details are aligned before diving into execution.

## Target Audience

Assume the primary reader of the task list is a **junior developer** who will implement the feature with awareness of the existing codebase context.

## Sync & Status Guidance
- The generated task list must be designed for immediate status updates during execution
- Each sub-task should be individually markable with `[ ]` → `[x]` and safe to skip if already `[x]`
- Include a clear "Execution Configuration" section so the processor knows when to pause and how to commit
- Encourage idempotent sub-tasks and atomic commits to avoid partial or duplicated work