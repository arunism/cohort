# Data folder

The Team Sprint Board app writes here. Each record is one JSON file, and every change is a commit, so the Git history doubles as an audit log.

| Folder | One file per | File name |
|---|---|---|
| `members/` | team member (name, role, level, status) | random id |
| `roles/` | role, with its list of levels | random id |
| `sprints/` | sprint (dates, goal, status, review notes, leave per person) | random id |
| `epics/` | epic (title, status, owner, dates, color) | `E-<number>.json` |
| `tasks/` | task (points to its epic with `epicId`, its sprint with `sprintId`) | `T-<number>.json` |
| `subtasks/` | subtask (points to its task with `taskId`) | `ST-<number>.json` |
| `analysis/` | workload settings (`config.json`) and review settings (`review.json`) | `config`, `review` |
| `reviews/` | one person's review for one sprint or period: rating, notes. **Private, keep the repo private.** | `<period>--<member id>` |
| `settings/` | team color palette (`palette.json`) | `palette` |
| `meetings/` | standup, product call or planning record | random id |
| `docs/` | Markdown doc (images, videos, Mermaid, code) | random id |

Uploaded images, videos and files go in `../assets/`.

You can edit these files by hand or with scripts. Keep them valid JSON; the app skips files it can't parse.
