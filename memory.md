# Codebase Memory: profile-redesign

## Phase 1 — Repository Discovery
- **Root structure**: A simple static repository representing a GitHub user profile README.
- **Applications**: None.
- **Packages**: None.
- **Services**: None.
- **Shared libraries**: None.
- **Infrastructure folders**: `.github/workflows/` containing GitHub Actions.
- **Documentation**: `README.md` (the main profile page), `SETUP.md` (integration guide).

## Phase 2 — Technology Detection
- **Frontend Framework**: None (GitHub Flavored Markdown).
- **Backend Framework**: None.
- **Database**: None.
- **Authentication**: None.
- **State Management**: None.
- **Styling**: None (Markdown embedded HTML for basic styling).
- **Infrastructure**: GitHub Actions (Cron jobs for automation).

## Phase 3 — Project Purpose Analysis
1. **What business problem is solved?**: Acts as a dynamic, automated portfolio and profile page for Aditya Bhure's GitHub account.
2. **What users use this?**: Recruiters, developers, and visitors to the GitHub profile.
3. **What are the major features?**: Auto-updating GitHub metrics, dynamic WakaTime stats, automated blog post fetching, and snake animation based on contribution graph.
4. **What is the user workflow?**: Users simply view the `README.md` on the GitHub profile.
5. **What are the primary entities?**: The `README.md` file itself.

## Phase 4 — Architecture Analysis
- **Frontend**: GitHub UI (Markdown rendering).
- **Backend / External Services**: GitHub Actions triggering external APIs (WakaTime, Metrics, Medium/Dev.to RSS).
- **Database**: None.
- **Authentication**: GitHub Secrets (`METRICS_TOKEN`, `WAKATIME_API_KEY`).
- **Cron Jobs**: 
  - `metrics.yml` (Sunday 00:00 UTC)
  - `blog-post-workflow.yml` (Daily 00:00 UTC)
  - `snake.yml` (Every 12 hours)
  - `update-profile.yml` (Sunday 00:00 UTC)

*Architecture Diagram:*
```
GitHub Actions (Cron) -> Fetch Data (APIs/RSS) -> Update README.md -> GitHub Commits -> Visitor views Profile
```

## Phase 5 — Routing Intelligence
- N/A. Single page profile.

## Phase 6 — Frontend Analysis
- **Entry point**: `README.md`
- **Data source**: Hardcoded JSON-like text, injected Markdown from Actions, embedded images (SVG generation).

## Phase 7 — Backend Analysis
- N/A. Handled via standalone GitHub Action YAML workflows.

## Phase 8 — Database Intelligence
- N/A.

## Phase 9 — Data Flow Analysis
- Time Trigger -> GitHub Action -> Action fetches 3rd-party data (Metrics/Blog/WakaTime) -> Action modifies `README.md` / creates SVGs -> Action commits to repository -> Updates live profile.
