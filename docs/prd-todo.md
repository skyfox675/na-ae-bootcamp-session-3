# Product Requirements Document (PRD) - TODO App Upgrade (MVP + Post-MVP)

## 1. Overview

We are upgrading the existing TODO app from a minimal task model to a more practical planning tool while keeping implementation simple and teachable. The immediate goal is to deliver a lean MVP focused on due dates, priority, and filtering, with no backend changes and local-only storage. Additional usability enhancements such as visual overdue emphasis and advanced sorting are intentionally deferred to Post-MVP.

---

## 2. MVP Scope

- Add `dueDate` to each task as an optional field.
- `dueDate` format must be ISO `YYYY-MM-DD`.
- Invalid `dueDate` values should be ignored and treated as no due date.
- Add `priority` to each task as an enum: `P1 | P2 | P3`.
- Default `priority` to `P3` when not provided.
- Keep `title` required in the task model.
- Add filters/tabs for task views:
  - All
  - Today
  - Overdue
- Keep storage local only (no backend or external storage integration).
- Do not introduce backend schema/API changes.

---

## 3. Post-MVP Scope

- Add visual highlighting for overdue tasks (for example, red emphasis) so overdue items stand out.
- Add deterministic sorting in this order:
  - Overdue tasks first
  - Then by priority (`P1` to `P3`)
  - Then by due date ascending
  - Tasks without due dates last

---

## 4. Out of Scope

- Notifications
- Recurring tasks
- Multi-user support
- Keyboard navigation enhancements
- External storage
- Any non-local persistence architecture
