# Cohort

Cohort is a sprint and team tracker that runs on GitHub alone. The app is a single web page; everything your team adds is stored as files in this repository:

- `data/` holds members, roles and levels, sprints, epics (E-1, E-2…), tasks (T-1, T-2…), subtasks (ST-1, ST-2…), meetings, docs and reviews, one JSON file each.
- `assets/` holds images, videos and files uploaded into docs.
- Every save is a commit, so you get full history and can roll back any change.

## What's in it

- **Help**: the top-bar **Help** tab opens the *Cohort guide*, a doc in the Docs section (`data/docs/cohort-guide.json`) that explains every page and field, with flow diagrams. Edit it like any doc. If it's missing or deleted, Help offers to add it back.
- **Epics → Tasks → Subtasks**, like Jira. Epics group related tasks and show progress (% of tasks done, plus subtask counts), dates, owner and color. Each task can belong to one epic.
- **Board swimlanes** (switch at the top of the Tasks board):
  - **Subtasks** (default): one lane per task that has subtasks, with its subtasks as cards in To do, In progress, On test and Done, and a progress bar. Drag subtask cards across their lane. When every subtask is done, the lane offers a button to finish the parent task. Tasks without subtasks share a lane at the bottom.
  - **Epics**: one lane per epic with its tasks, plus a "No epic" lane.
  - **None**: the flat board with an Overdue column.
  - Lanes can be collapsed; the choice is remembered per browser.
- **Tasks**: view by sprint (active sprint by default), quarter (current by default) or the last 7 days, and filter by epic, member, role, level, priority and deadline. Cards turn orange once 90% of the time from start to due date is used, and move to Overdue when the due date passes.
- **Subtasks** with their own numbering, status, assignee and estimate.
- **Workload** page for sprint and quarter planning. Press **Run analysis** to see, for every person and role:
  - load (planned effort ÷ capacity), free capacity, open and in-progress work, overdue items, due-date clusters, carry-over, leave and a stress score
  - findings from the rules, grouped by severity
  - suggested reassignments from overloaded people to people with room in the same role, with one-click **Assign**
  - **Fields**, **Rules** and **Assumptions** tabs where you add, edit, reorder, turn off or delete what's analyzed. Fields and rules use simple formulas such as `load > 100` or `levelFactor < 0.75 and highPriority >= 3`. Reset to defaults at any time.
- **Reviews** page with two modes, both run with one click on built-in defaults:
  - **Sprint retro**: picks the last completed sprint (or any sprint; a **Retro** button also sits on each sprint card and in the archive). Shows each person's completed and unfinished work, effort against what their level should deliver, on-time rate, carry-over, cycle time, high-priority work and meeting attendance, then findings marked **Strength**, **Watch**, **Needs support** or **Note**. Write a private note per person (rating 1–5, what went well, what to improve, commitments) and team retro notes saved with the sprint.
  - **Performance review**: quarterly (pick a quarter), half-yearly (the 6 months up to today or a date you choose) or yearly (12 months up to that date). The same per-person view, plus each person's review history.
  - **Fields**, **Rules** and **Assumptions** tabs work like Workload but are saved separately (`data/analysis/review.json`), so changing review rules never changes planning rules. Rules can target sprint retros, all reviews, or only quarterly, half-yearly or yearly ones, and can use the rating you gave (for example `rating >= 4`).
- **Sprints & backlog**: create, start and complete sprints (including ending one early). Unfinished work moves to the next sprint or the backlog.
- **Team** with active or inactive status, and **Roles & levels** you manage yourself. Members can only be given existing roles and levels.
- **Colors** for members and epics: 20 presets including black, white and grays, plus **+** to pick any color. Picked colors are saved to a shared team palette (`data/settings/palette.json`) and can be removed from it; avatar and label text switches to dark on light colors.
- **Meetings**: daily standups, weekly product calls, and sprint or quarter planning, with templates.
- **Docs**: Markdown with live preview, image and video uploads, Mermaid diagrams, code highlighting and optional links to tasks.
- **Archive**: done tasks, done epics, completed sprints and inactive members, all filterable.
- Deleting anything asks you to type a short word shown on screen (such as POOF or ZAP) and press Enter. Letters light up as you type; a wrong letter shakes the word.

## Set up (about 10 minutes)

1. **Create a repository** on GitHub and push these files to the `main` branch.
   ```bash
   cd cohort
   git init -b main
   git add .
   git commit -m "Add Cohort"
   git remote add origin https://github.com/<owner>/<repo>.git
   git push -u origin main
   ```
2. `config.js` is already set to `arunism/cohort` on `main`. Change it if you move the repository.
3. **Turn on Pages:** in the repository go to **Settings → Pages → Build and deployment → Source** and choose **GitHub Actions**. The included workflow publishes the app; open the **Actions** tab to watch it and get the site address (`https://<owner>.github.io/<repo>/`).
4. **Each teammate creates a token:** GitHub → Settings → Developer settings → **Fine-grained personal access tokens** → Generate new token.
   - Repository access: **Only select repositories** → this repository.
   - Permissions: **Contents → Read and write** (or **Read-only** for people who should only view).
   - Copy the token, open the site and paste it in. It stays in that browser's local storage and is sent only to `api.github.com`.

That's it. Start on **Team → Roles & levels**, then add members, create your first sprint, and add epics for your bigger pieces of work.

## Privacy: who can see your data

- The Pages site contains only the app (`index.html` and `config.js`). Your records are never copied to it; the app loads them through the GitHub API with each person's token.
- So **who can see the data is decided by the repository's visibility**:
  - **Private repository**: only people with access to the repo (and a token for it) can read or change anything. Publishing Pages from a private repository needs a paid plan (GitHub Pro, Team or Enterprise). The app page itself will still be publicly reachable, but it shows nothing without a token.
  - **Public repository**: works on the free plan, but everything in `data/` and `assets/` is public on GitHub.
- **No Pages at all:** with a private repository on the free plan, teammates can clone the repo and open `index.html` straight from their computer. It works the same way and saves to GitHub.

## Good to know

- **Live updates**: the app checks for teammates' changes every 30 seconds and when you return to the tab.
- **Simultaneous edits**: if two people change the same record at once, the second save re-reads the latest version and applies its change on top. If two people create a task at the same moment, the second one gets the next free number.
- **Workload settings** are saved in `data/analysis/config.json`. Until you change anything, the built-in defaults are used. Capacity uses each person's level factor (junior 0.65, mid and senior 0.85, lead 0.7 by default), working days minus weekends, public holidays and leave, and a buffer (20% per sprint, 25% per quarter). Items without an estimate count as 3 points. All of these are editable.
- **Numbering**: the next epic, task or subtask number is one past the highest existing one. If you delete the newest item, its number can be used again.
- **Uploads** are limited to 25 MB each. GitHub warns about files over 50 MB and rejects files over 100 MB, so link long recordings (YouTube, Loom) instead of uploading them.
- **Busy history**: every change is a commit by the person who made it. The workflow ignores `data/` and `assets/`, so saves don't redeploy the site.
- **API limits**: GitHub allows 5,000 API requests per hour per token. Normal use stays far below that: one small check every 30 seconds, plus one request per save.
- **Offline**: the app needs to reach `api.github.com`. Fonts and the Markdown, diagram and code-highlighting libraries load from public CDNs.

## Files

```
index.html                    the app
config.js                     optional default owner / repo / branch
.github/workflows/pages.yml   publishes the app to GitHub Pages
.github/dependabot.yml        keeps the workflow's actions up to date
data/                         records written by the app (see data/README.md)
assets/                       uploads written by the app
```

## Privacy of reviews

Review notes and ratings are stored as plain JSON in `data/reviews/`. **Anyone who can read the repository can read them**, including everyone on the internet if the repository is public. Keep the repository private if you write performance reviews here, and limit who has read access. The published Pages site itself contains no data, but anyone with a token for the repo can load it.


## Upgrading from Team Sprint Board

Cohort is the same app under a new name, and your data is unchanged. When upgrading an existing repository:

- Replace `index.html`, `config.js`, `README.md`, `data/README.md` and `.github/workflows/pages.yml`.
- Add `data/docs/cohort-guide.json`, or open **Help** and press **Add the guide to Docs**.
- Teammates stay signed in, because saved tokens and board settings are carried over automatically. An old `config.js` that still uses `TSB_CONFIG` keeps working too.
