# Team Sprint Board

A sprint and team tracker that runs on GitHub alone. The app is a single web page; everything your team adds is stored as files in this repository:

- `data/` holds members, roles and levels, sprints, tasks (T-1, T-2…), subtasks (ST-1, ST-2…), meetings and docs, one JSON file each.
- `assets/` holds images, videos and files uploaded into docs.
- Every save is a commit, so you get full history and can roll back any change.

## What's in it

- **Tasks**: board with To do, In progress, On test and Overdue columns. View by sprint (active sprint by default), quarter (current by default) or the last 7 days, and filter by member, role, level, priority and deadline. Cards turn orange once 90% of the time from start to due date is used, and move to Overdue when the due date passes.
- **Subtasks** with their own numbering, status and assignee.
- **Sprints & backlog**: create, start and complete sprints (including ending one early). Unfinished work moves to the next sprint or the backlog.
- **Team** with active or inactive status, and **Roles & levels** you manage yourself. Members can only be given existing roles and levels.
- **Meetings**: daily standups, weekly product calls, and sprint or quarter planning, with templates.
- **Docs**: Markdown with live preview, image and video uploads, Mermaid diagrams, code highlighting and optional links to tasks.
- **Archive**: done tasks, completed sprints and inactive members, all filterable.
- Deleting anything asks the person to draw a shape or solve a picture puzzle first.

## Set up (about 10 minutes)

1. **Create a repository** on GitHub and push these files to the `main` branch.
   ```bash
   cd team-sprint-board
   git init -b main
   git add .
   git commit -m "Add Team Sprint Board"
   git remote add origin https://github.com/<owner>/<repo>.git
   git push -u origin main
   ```
2. **Optional:** edit `config.js` with your owner, repo and branch so teammates only have to paste a token.
3. **Turn on Pages:** in the repository go to **Settings → Pages → Build and deployment → Source** and choose **GitHub Actions**. The included workflow publishes the app; open the **Actions** tab to watch it and get the site address (`https://<owner>.github.io/<repo>/`).
4. **Each teammate creates a token:** GitHub → Settings → Developer settings → **Fine-grained personal access tokens** → Generate new token.
   - Repository access: **Only select repositories** → this repository.
   - Permissions: **Contents → Read and write** (or **Read-only** for people who should only view).
   - Copy the token, open the site and paste it in. It stays in that browser's local storage and is sent only to `api.github.com`.

That's it. Start on **Team → Roles & levels**, then add members, then create your first sprint.

## Privacy: who can see your data

- The Pages site contains only the app (`index.html` and `config.js`). Your records are never copied to it; the app loads them through the GitHub API with each person's token.
- So **who can see the data is decided by the repository's visibility**:
  - **Private repository**: only people with access to the repo (and a token for it) can read or change anything. Publishing Pages from a private repository needs a paid plan (GitHub Pro, Team or Enterprise). The app page itself will still be publicly reachable, but it shows nothing without a token.
  - **Public repository**: works on the free plan, but everything in `data/` and `assets/` is public on GitHub.
- **No Pages at all:** with a private repository on the free plan, teammates can clone the repo and open `index.html` straight from their computer. It works the same way and saves to GitHub.

## Good to know

- **Live updates**: the app checks for teammates' changes every 30 seconds and when you return to the tab.
- **Simultaneous edits**: if two people change the same record at once, the second save re-reads the latest version and applies its change on top. If two people create a task at the same moment, the second one gets the next free number.
- **Numbering**: the next task number is one past the highest existing task. If you delete the newest task, its number can be used again.
- **Uploads** are limited to 25 MB each. GitHub warns about files over 50 MB and rejects files over 100 MB, so link long recordings (YouTube, Loom) instead of uploading them.
- **Busy history**: every change is a commit by the person who made it. The workflow ignores `data/` and `assets/`, so saves don't redeploy the site.
- **API limits**: GitHub allows 5,000 API requests per hour per token. Normal use stays far below that: one small check every 30 seconds, plus one request per save.
- **Offline**: the app needs to reach `api.github.com`. Fonts and the Markdown, diagram and code-highlighting libraries load from public CDNs.

## Files

```
index.html                    the app
config.js                     optional default owner / repo / branch
.github/workflows/pages.yml   publishes the app to GitHub Pages
data/                         records written by the app (see data/README.md)
assets/                       uploads written by the app
```
