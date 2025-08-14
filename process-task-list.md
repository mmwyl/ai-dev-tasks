# Task List Management

Guidelines for managing task lists in markdown files to track progress on completing a PRD

## Task Implementation

### Execution Modes

**Default Mode (Step-by-step):**
- **One sub-task at a time:** Do **NOT** start the next sub‑task until you ask the user for permission and they say "yes" or "y"

**Auto Mode (Continuous execution):**
- When the user explicitly requests "auto mode" or "continuous execution", you may proceed through all sub-tasks without stopping for confirmation between each one
- **Still pause for parent task completion:** Always stop after completing a parent task (when all its sub-tasks are done) to allow user review before proceeding to the next parent task
- **Quality gates remain:** All quality checks, tests, and commit protocols still apply
- **Completion protocol:**  
  1. When you finish a **sub‑task**, immediately mark it as completed by changing `[ ]` to `[x]`.
  2. If **all** subtasks underneath a parent task are now `[x]`, follow this sequence:
    - **First**: Run the project's full test suite using the command configured in the project
    - **Quality Check**: Verify implementation against the Detail Checklist in the task file:
      - [ ] Edge cases and error flows are handled
      - [ ] External integrations work with correct connection points
      - [ ] Data validation/transformation/persistence operates correctly
      - [ ] Performance considerations are implemented
      - [ ] Security concerns are addressed
      - [ ] UX details function as expected
    - **Only if all tests pass AND checklist is verified**: Stage changes (`git add .`)
    - **Clean up**: Remove any temporary files and temporary code before committing
    - **Commit**: Use a descriptive commit message that:
      - Uses conventional commit format (`feat:`, `fix:`, `refactor:`, etc.)
      - Summarizes what was accomplished in the parent task
      - Lists key changes and additions
      - References the task number and PRD context
      - **Formats the message as a single-line command using `-m` flags**, e.g.:

        ```
        git commit -m "feat: add payment validation logic" -m "- Validates card type and expiry" -m "- Adds unit tests for edge cases" -m "Related to T123 in PRD"
        ```
  3. Once all the subtasks are marked completed and changes have been committed, mark the **parent task** as completed.
- Stop after each sub‑task and wait for the user's go‑ahead.

## Sync Policy and Idempotency

To prevent task state desynchronization and ensure reliable execution:

### Immediate Status Updates
- **Update task status immediately** after completing each sub-task by changing `[ ]` to `[x]` in the task list file
- **Save the file immediately** after each status change to persist the state
- **Never batch status updates** - mark completion as soon as the sub-task is done

### State Verification Before Execution  
- **Always check task status** before starting any sub-task
- **Skip tasks already marked `[x]`** to avoid duplicate work
- **Verify parent task completion** by ensuring all sub-tasks are `[x]` before marking the parent as complete

### Idempotency Requirements
- **Re-execution safe**: Each sub-task should be designed to be safely re-runnable
- **State checks**: Before executing, verify if the desired outcome already exists
- **Atomic commits**: Group related changes into single commits to maintain consistency

### Recovery Protocol
- If task list becomes out of sync with actual progress:
  1. Review completed work in the codebase
  2. Update task status to reflect actual completion state
  3. Document any discrepancies found
  4. Resume from the correct next task

## Task List Maintenance

1. **Update the task list as you work:**
   - Mark tasks and subtasks as completed (`[x]`) per the protocol above.
   - Add new tasks as they emerge.

2. **Maintain the "Relevant Files" section:**
   - List every file created or modified.
   - Give each file a one‑line description of its purpose.

## AI Instructions

When working with task lists, the AI must:

1. Regularly update the task list file after finishing any significant work.
2. Follow the completion protocol:
   - Mark each finished **sub‑task** `[x]`.
   - Mark the **parent task** `[x]` once **all** its subtasks are `[x]`.
3. Add newly discovered tasks.
4. Keep "Relevant Files" accurate and up to date.
5. Before starting work, check which sub‑task is next.
6. After implementing a sub‑task, update the file and then pause for user approval. If the user has enabled Auto Mode, do not pause between sub‑tasks under the same parent task; still pause after the parent task is completed for review.
7. Before staging or committing, self-check against the "Detail Checklist" in the task file and resolve any gaps.
8. Detect the project's established commands and tools for: running tests, building/compiling, and quality gates (linters/formatters/static analysis). Use those consistently.