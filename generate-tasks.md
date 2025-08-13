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
3.  **Assess Current State:** Review the existing codebase to understand existing infrastructre, architectural patterns and conventions. Also, identify any existing components or features that already exist and could be relevant to the PRD requirements. Then, identify existing related files, components, and utilities that can be leveraged or need modification.
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

- `src/main/java/com/example/ui/MainFrame.java` - The main SwingX JFrame (UI entry point) assembling panels and menus.
- `src/main/java/com/example/ui/ImportPanel.java` - SwingX panel for selecting files and triggering import actions.
- `src/main/java/com/example/service/ImportService.java` - Business logic for parsing and validating Excel/CSV files.
- `src/main/java/com/example/integration/excel/EasyExcelImporter.java` - EasyExcel-based reader for `.xlsx`.
- `src/main/java/com/example/integration/csv/OpenCsvImporter.java` - OpenCSV-based reader for `.csv`.
- `src/main/java/com/example/model/Record.java` - Data model for imported rows.
- `src/test/java/com/example/service/ImportServiceTest.java` - Unit tests for `ImportService` (JUnit 5 + Mockito).
- `src/test/java/com/example/integration/excel/EasyExcelImporterTest.java` - Unit tests for EasyExcel importer.
- `src/test/java/com/example/integration/csv/OpenCsvImporterTest.java` - Unit tests for OpenCSV importer.

### Notes

- Unit tests should typically be placed in `src/test/java` following the same package structure as the main code (e.g., `UserService.java` in `src/main/java/com/example/feature` and `UserServiceTest.java` in `src/test/java/com/example/feature`).
- Use `mvn test` to run all tests, or `mvn -Dtest=UserServiceTest test` to run a specific test class. Running without `-Dtest` executes all tests found by the Maven Surefire configuration.

#### SwingX + File Import Best Practices

- **UI 分层原则**：SwingX 组件应专注于渲染与事件绑定，业务逻辑应抽取到 Service 层。ActionListener 中仅转发用户操作到相应的 Service 方法，便于单元测试。
- **文件选择与校验**：使用 `JFileChooser` 进行文件选择，设置 `FileNameExtensionFilter` 限制扩展名（如 `.xlsx`, `.csv`），并在打开前验证 MIME 类型或文件头，防止恶意文件。
- **大文件内存策略**：
  - **EasyExcel**：使用 `AnalysisEventListener` 逐行读取，避免将整个文件加载到内存。设置合理的批次大小（如每 1000 行处理一次）。
  - **OpenCSV**：使用 `CSVReader` 的迭代器模式或 `CSVIterator`，逐行流式读取，避免 `readAll()` 方法。
- **错误收集机制**：区分"致命错误"（文件格式错误、IO 异常）和"数据错误"（某行字段校验失败）。对数据错误采用收集模式，记录行号与错误信息，允许用户查看完整的错误报告。
- **UI 可测试性**：将 Swing 组件的业务逻辑抽取为 Presenter 或 Controller 类，UI 类仅负责组件初始化、布局和事件绑定。这样可以对 Presenter 进行单元测试，而无需启动 GUI 环境。
- **导入器测试策略**：对 EasyExcel/OpenCSV 导入器的单元测试可使用内存中的临时文件或 `ByteArrayInputStream`，避免依赖真实文件系统，提高测试的可重复性和执行速度。

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