# Cloud Architecture Overview

## System Context

```mermaid
flowchart LR
    user[Todo app user]
    frontend[React frontend\npackages/frontend]
    api[Express API\npackages/backend]
    store[(In-memory SQLite store\nTask data)]

    user -->|Uses in a web browser| frontend
    frontend -->|HTTP requests to /api/tasks| api
    api -->|Reads and writes tasks| store
```

The repository is an npm-workspaces monorepo. The React frontend provides the browser experience and calls the Express API. The API persists task data only in its in-memory SQLite store, so data is lost when the API process restarts.

## Create a TODO

```mermaid
sequenceDiagram
    actor User
    participant Frontend as React frontend
    participant API as Express API
    participant Store as In-memory SQLite store

    User->>Frontend: Enter TODO details and submit
    Frontend->>API: POST /api/tasks with task JSON
    API->>Store: Insert task
    Store-->>API: Created task record
    API-->>Frontend: 201 Created with task JSON
    Frontend->>API: GET /api/tasks
    API->>Store: Query tasks
    Store-->>API: Task list
    API-->>Frontend: 200 OK with task list
    Frontend-->>User: Display the new TODO
```