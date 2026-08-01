# GitHub Profile README — gittydia

## Overview

Create a GitHub profile README for user `gittydia` modeled after `Andrew6rant/Andrew6rant`. The profile displays personal info, contact details, tech stack, hobbies, and auto-updating GitHub stats in a terminal-themed SVG rendered via light/dark mode detection.

## Repository

- **Name:** `gittydia/gittydia` (special GitHub profile repository)
- **Visibility:** Public

## Architecture

```
gittydia/gittydia/
├── .github/workflows/build.yaml   # Scheduled + push-triggered Actions workflow
├── cache/
│   ├── requirements.txt           # Python dependencies
│   └── <hash>.txt                 # Per-repo LOC cache (keyed by username hash)
├── today.py                       # Python script: queries GitHub GraphQL API, updates SVGs
├── README.md                      # Markdown with <picture> for dark/light mode switching
├── light_mode.svg                 # Light theme SVG (auto-generated)
└── dark_mode.svg                  # Dark theme SVG (auto-generated)
```

### Data Flow

1. GitHub Actions workflow triggers (scheduled 4AM UTC daily + on push to `main`)
2. `today.py` runs with `ACCESS_TOKEN` and `USER_NAME` environment variables
3. Script queries GitHub GraphQL v4 API for: repos, stars, followers, commits, lines of code
4. LOC data is cached per-repo in `cache/<hash>.txt` to minimize API calls
5. Both `light_mode.svg` and `dark_mode.svg` are updated with new stats via XML element replacement
6. Changes are committed and pushed back to `main`

### Key Dependencies

- `python-dateutil` — relative date calculations (age/uptime)
- `requests` — GitHub GraphQL API calls
- `lxml` — SVG XML parsing and element replacement

## SVG Layout

Dimensions: 985px × 530px, font-family Consolas, font-size 16px.

### Layout Columns

- **Left (x: 15–380):** Terminal-themed ASCII art (code/terminal style)
- **Right (x: 390–985):** System-info-style panel

### Right Panel Content

```
gittydia@grant ─────────────────────────────────────────
  OS: Windows + Linux Mint + Android
  Uptime: XX years, XX months, XX days
  Host: Rizal Technological University
  Kernel: student + learner + software engineer
  IDE: VSCode + Antigravity

  Languages.Programming: JavaScript, TypeScript, Python
  Languages.Frameworks: React, Next.js, Tailwind CSS, React Native, Vite
  Languages.Backend: Django, FastAPI, Flask
  Languages.Databases: PostgreSQL, MySQL, MongoDB, SQLite, Firebase, Supabase
  Languages.DevOps: Docker, Vercel
  Languages.Real: English, Filipino

  Hobbies: Coding, Gaming, Reading

─ Contact ──────────────────────────────────────────────
  Email.Personal: boholstdianne1@gmail.com
  LinkedIn: dianne-boholst
  Discord: ianne1

─ GitHub Stats ─────────────────────────────────────────
  Repos: XX {Contributed: XX} | Stars: XX
  Commits: XX | Followers: XX
  Lines of Code: XX,XXX (XXX,XXX++, XXX,XXX--)
```

### Theming

- **Light mode** (`light_mode.svg`): Background `#f6f8fa`, text `#24292f`, keys `#953800`, values `#0a3069`, additions `#1a7f37`, deletions `#cf222e`
- **Dark mode** (`dark_mode.svg`): Background `#161b22`, text `#c9d1d9`, keys `#ffa657`, values `#a5d6ff`, additions `#3fb950`, deletions `#f85149`
- README uses `<picture>` element with `prefers-color-scheme: dark` media query

## `today.py` Adaptations

Changes from Andrew6rant's original:

| Item | Andrew6rant | gittydia |
|------|------------|----------|
| Birthday | 2002-07-05 | 2005-05-18 |
| Archive repos | Yes (add_archive) | Skip (no archived data) |
| OWNER_ID check | MDQ6VXNlcjU3MzMxMTM0 | N/A (skip add_archive) |
| SVG text updates | Same structure | Adapted for new labels |

The `add_archive()` function and its associated `OWNER_ID` check will be removed since gittydia has no archived repository data.

## GitHub Actions

### Workfile: `.github/workflows/build.yaml`

- **Trigger:** `push` to `main` + `schedule` at `0 4 * * *` (daily 4AM UTC)
- **Runner:** `ubuntu-latest`
- **Python:** 3.8

### Secrets Required

| Secret | Value |
|--------|-------|
| `ACCESS_TOKEN` | Fine-grained PAT with repo & account permissions |
| `USER_NAME` | `gittydia` |

### Permissions for ACCESS_TOKEN

- Account permissions: read:Followers, read:Starring
- Repository permissions: read:Commit statuses, read:Contents, read:Metadata, read:Issues, read:Pull Requests

## Error Handling

- API rate limits: Cache file saves partial data before crash if `recursive_loc` fails with 403
- Empty repositories: Skipped during LOC calculation
- SVG element not found: Skipped gracefully (no crash if ID missing)

## Testing

- No formal test suite; verification by running `today.py` locally and checking SVG output
- GitHub Actions log shows execution time for each query function and total API call count
