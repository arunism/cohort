# Data folder

The Team Sprint Board app writes here. Each record is one JSON file, and every change is a commit, so the Git history doubles as an audit log.

| Folder | One file per | File name |
|---|---|---|
| `members/` | team member (name, role, level, status) | random id |
| `roles/` | role, with its list of levels | random id |
| `sprints/` | sprint (dates, goal, status, review notes) | random id |
| `tasks/` | task | `T-<number>.json` |
| `subtasks/` | subtask (points to its task with `taskId`) | `ST-<number>.json` |
| `meetings/` | standup, product call or planning record | random id |
| `docs/` | Markdown doc (images, videos, Mermaid, code) | random id |

Uploaded images, videos and files go in `../assets/`.

You can edit these files by hand or with scripts. Keep them valid JSON; the app skips files it can't parse.
