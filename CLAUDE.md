# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

TRUST-AI is a client-side web application for generating AI governance manifests. It guides organizations through a 7-step workflow to assess AI use cases and generate a compliance/best-practices manifest. There is no build system, no backend, and no npm dependencies — just static HTML files served directly in the browser.

## Development

No build step required. Open any `.html` file directly in a browser, or serve with any static file server:

```bash
# Simple local server
python3 -m http.server 8080
# or
npx serve .
```

The entry point is `index.html`, which redirects to `trust-ai-screen7.html`.

## Architecture

### Application Flow (7 screens)

| File | Screen | Purpose |
|------|--------|---------|
| `trust-ai-screen7.html` | Dashboard | Entry point — project list, create/edit/delete projects |
| `trust-ai-screen1.html` | Project Creation | Project metadata form + optional CSV tool upload |
| `trust-ai-screen3.html` | Use Case Discovery | Kano model funnel — browse & select from 24 predefined use cases |
| `trust-ai-screen4.html` | Shortlist & Prioritization | Reorder selected use cases |
| `trust-ai-screen5.html` | Assessment | Multi-step rating form per use case — triggers flags/warnings |
| `trust-ai-screen6.html` | Manifest Generation | Output document with best practices, flags, PDF export |

### State Management

All application state lives in `localStorage`:
- `trust_ai_projects` — array of all project metadata
- `trust_ai_project` — the currently active project object

**Project object shape:**
```js
{
  id, name, description, pm_email,
  industry: [],       // array of selected industries
  projectType,        // IT | Org | Product | Other
  tools,              // uploaded CSV inventory
  status,             // discovery | assessment | completed
  shortlist,          // selected use case IDs
  assessments_map,    // { [ucId]: assessmentData }
  manifest_needs_refresh,
  assessments_need_industry_review
}
```

### Key Data Structures (defined inline in screen files)

- `USE_CASES` / `ALL_UCS` — 24 predefined use cases with IDs, titles, and metadata
- `BEST_PRACTICES` — guidance mapped per use case ID
- `MEASURES` — evaluation criteria shown during assessment
- `FLAGS` — risk/warning flags triggered by assessment answers
- `INDUSTRY_TO_REGULATION` — maps industry selection to regulatory requirements

### Theming

Light/dark mode via CSS custom properties. Theme preference persisted in `localStorage` as `trust_ai_theme`.

## Tech Stack

- Vanilla HTML5 / CSS3 / ES6+ JavaScript — no frameworks
- DM Sans (body font) + DM Mono (monospace)
- Browser APIs: `localStorage`, `FileReader`, `Clipboard`, `window.print()` for PDF

## GitHub

Remote: `waxxdu/trust-ai-prototype`

## Feature Backlog

See `../Verbesserungsideen App.md` (German) for tracked UX improvements and known issues.
