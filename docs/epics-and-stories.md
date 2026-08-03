# TODO App Epics and Stories

Based on the requirements in `docs/prd-todo.md` and structured according to `docs/templates/epic-and-stories-template.md`.

Technical requirements must reference the current frontend implementation in `packages/frontend/src` and backend implementation in `packages/backend/src`; the PRD prohibits backend schema or API changes.

## MVP

- Epic: Task Planning Details
  - Story: Add an optional due date to tasks
    - Acceptance Criteria:
      - Tasks support an optional `dueDate` field.
    - Technical Requirements:
      - Extend the frontend task state and request payloads in `TaskForm.js`, `App.js`, and `TaskList.js` to use an optional `dueDate` value.
      - Do not change the existing backend `due_date` schema or API contract in `packages/backend/src/app.js`.
  - Story: Add priority levels to tasks
    - Acceptance Criteria:
      - Tasks support `P1`, `P2`, and `P3` priority values only.
    - Technical Requirements:
      - Add a priority control in `TaskForm.js` that limits values to `P1`, `P2`, and `P3`.
      - Maintain priority in frontend task state without changing the backend schema or API.
  - Story: Default task priority to P3
    - Acceptance Criteria:
      - A task without a provided priority is assigned `P3`.
    - Technical Requirements:
      - Initialize the priority state in `TaskForm.js` to `P3` for new tasks and when an existing task has no priority.
  - Story: Keep task titles required
    - Acceptance Criteria:
      - A task must have a title.
    - Technical Requirements:
      - Preserve the required title input and trimmed-title validation in `TaskForm.js`.
      - Preserve the existing title validation in the `POST` and `PUT` handlers in `packages/backend/src/app.js`.

- Epic: Due Date Validation
  - Story: Accept due dates in YYYY-MM-DD format
    - Acceptance Criteria:
      - A provided `dueDate` is accepted only when it uses the ISO `YYYY-MM-DD` format.
    - Technical Requirements:
      - Use the date input in `TaskForm.js` to submit dates in `YYYY-MM-DD` format.
      - Validate the date string in frontend task handling before it is included in a request.
  - Story: Treat invalid due dates as no due date
    - Acceptance Criteria:
      - An invalid `dueDate` is ignored.
      - A task with an invalid `dueDate` is treated as having no due date.
    - Technical Requirements:
      - Normalize an invalid or absent date to an empty value in `TaskForm.js` before saving the task.
      - Render an absent due date in `TaskList.js` without a due-date chip.

- Epic: Task Date Views
  - Story: Add an All tasks view
    - Acceptance Criteria:
      - Users can select an All tasks view.
    - Technical Requirements:
      - Add an All view control in the frontend and use it as the default `TaskList.js` view.
      - Filter the already loaded task collection in the frontend without modifying `GET /api/tasks`.
  - Story: Add a Today tasks view
    - Acceptance Criteria:
      - Users can select a Today tasks view.
    - Technical Requirements:
      - Add a Today view control that displays tasks whose valid `dueDate` equals the local current date.
      - Use the same date-only comparison format as the `TaskForm.js` input.
  - Story: Add an Overdue tasks view
    - Acceptance Criteria:
      - Users can select an Overdue tasks view.
    - Technical Requirements:
      - Add an Overdue view control that displays tasks with a valid `dueDate` before the local current date.
      - Exclude tasks without a due date from the Overdue view.

- Epic: Local-Only Task Storage
  - Story: Keep task data stored locally
    - Acceptance Criteria:
      - Task data is stored locally only.
      - The application does not use backend or external storage integration.
      - The application does not introduce backend schema or API changes.
    - Technical Requirements:
      - Keep task-view and planning metadata in frontend state only.
      - Do not add storage providers, external service calls, database migrations, or API endpoint changes.
      - Continue using the existing frontend fetch calls and Express endpoints without changing their request or response contracts.

## Post-MVP

- Epic: Overdue Task Visibility
  - Story: Visually highlight overdue tasks
    - Acceptance Criteria:
      - Overdue tasks have visual emphasis that makes them stand out.
    - Technical Requirements:
      - Add a conditional overdue style to each `TaskList.js` item when its valid `dueDate` is before the local current date.
      - Keep completed-state styling intact when applying overdue emphasis.

- Epic: Task Ordering
  - Story: Sort overdue tasks before other tasks
    - Acceptance Criteria:
      - Overdue tasks appear before non-overdue tasks.
    - Technical Requirements:
      - Sort the task collection in `TaskList.js` using an overdue status derived from valid date-only values before rendering.
  - Story: Sort tasks by priority from P1 to P3
    - Acceptance Criteria:
      - Within the applicable ordering group, tasks are ordered by priority from `P1` through `P3`.
    - Technical Requirements:
      - Use an explicit frontend priority rank of `P1`, `P2`, then `P3` as the second sort key.
  - Story: Sort tasks by due date ascending
    - Acceptance Criteria:
      - Within the applicable ordering group, tasks with due dates are ordered from earliest to latest.
    - Technical Requirements:
      - Compare valid `YYYY-MM-DD` values as the third sort key in ascending order.
  - Story: Place tasks without due dates last
    - Acceptance Criteria:
      - Tasks without due dates appear after tasks with due dates.
    - Technical Requirements:
      - Treat a missing or invalid `dueDate` as the last sort key category after dated tasks.