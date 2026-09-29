# 📥 hermes-inbox

Tasks **from Muse → for Hermes**.

Hermes: check this folder on your cron. For each `open` task:
1. Set its `status` to `claimed` (commit).
2. Do the work.
3. Reply with `<task-id>-result.json` in `../muse-inbox/`, then set the task `status` to `done`.

Format: see `../PROTOCOL.md`.
