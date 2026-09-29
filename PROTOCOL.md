# 🤝 Muse ↔ Hermes Bridge Protocol

> **Purpose:** Two-way collaboration between **Muse** (Meta's personal AI assistant, works with Jahanzaib in chat)
> and **Hermes** (AI agent running on Jahanzaib's Hostinger VPS). Both share capabilities through this repo.

## 📬 Mailboxes

| Direction | Folder |
|---|---|
| Muse → Hermes (tasks for Hermes) | `hermes-inbox/` |
| Hermes → Muse (tasks for Muse) | `muse-inbox/` |
| Capability manifests | `capabilities/` |
| Large deliverables (referenced from results) | `shared/` |

## 📝 Task file format (JSON)

```json
{
  "id": "muse-20260929-001",
  "from": "muse",
  "created_at": "2026-09-29T10:00:00+05:00",
  "type": "task",
  "title": "Short title",
  "body": "Full instructions, context, and what 'done' looks like.",
  "status": "open"
}
```

- `type`: `task` | `question` | `note`
- `status`: `open` → `claimed` → `in_progress` → `done` (or `failed` with the error log in `result`)

## 📥 Replying

The worker writes a result file **in the other agent's inbox**:

- Hermes replies in: `muse-inbox/<id>-result.json`
- Muse replies in: `hermes-inbox/<id>-result.json`

```json
{
  "reply_to": "<original id>",
  "from": "hermes",
  "created_at": "...",
  "status": "done",
  "result": "What was done, key outputs, file paths of deliverables."
}
```

## 📏 Rules

1. **Never edit the other agent's inbox files**, except the worker updates its own task's `status`.
2. Keep task files small. Large deliverables go in `shared/` — reference the path in `result`.
3. No secrets in task files. Ever. (Tokens, API keys, passwords → never in git.)
4. Poll cadence: Hermes checks `hermes-inbox/` on its own cron; Muse checks `muse-inbox/` every ~2 hours. Instant pings via `.github/workflows/ping-bridge.yml` are wake-up signals only — the worker still pulls the repo and reads the task file.
5. If a task is unclear, reply with `status: "question"` instead of guessing.
6. Jahanzaib's standing rules apply to both: no sending emails/messages from his accounts without his explicit approval; Roman Urdu by default with him.

## 🔌 Capabilities

Each agent keeps an honest manifest at `capabilities/<agent>.json` with `can[]`, `cannot[]`, and `poll_cadence`.
Read the other's manifest before assigning work it cannot do.
