<div align="center">
	<h1>Stack Collective</h1>
	<p><strong>Alumni knowledge & opportunity hub for graduates of the former Turing School of Software & Design</strong></p>
	<p>Community platform focused on professional visibility, connection, and continuous learning.</p>
	<img src="public/icon.png" alt="Stack Collective Icon" width="96" height="96" />
</div>

---

## Overview
Stack Collective is an early-stage Rails 8 application serving as a centralized dashboard for alumni. The landing page currently showcases a product direction: authentication, personalized dashboards, employment history tracking, collaborative news, and blog link sharing. Dynamic data wiring and persistence layers will follow in subsequent iterations.

## Current Status
Status: UI/UX scaffolding implemented (hero, feature roadmap, directory preview, news/blog placeholders, navigation, footer). No authentication, persistence models, or background jobs wired yet. Architecture intentionally lean to enable rapid iteration.

## Core Objectives
- Centralize alumni profiles (name, cohort code, current employer, historical employers)
- Provide discoverability via a searchable directory
- Facilitate professional storytelling through shared articles
- Surface community announcements and opportunities
- Support future personalization and contribution access through GitHub authentication

## Implemented (MVP Landing Layer)
- Responsive navigation bar with anchor links
- Hero section with community snapshot placeholder
- Feature roadmap card grid
- Directory table skeleton with placeholder shimmer rows
- News and Blogs sections with placeholder cards
- Gradient call-to-action section for future GitHub sign-in
- Themed footer and scoped design enhancements

## Planned Roadmap (High-Level)
| Area | Milestone | Notes |
|------|-----------|-------|
| Authentication | GitHub OAuth | OmniAuth or `omniauth-github` integration with state CSRF protection |
| Profiles | User model & profile editing | Employment history normalization (User, Employment, Company) |
| Directory | Search & filter | Cohort/year filter, employer filter, pagination |
| Employment History | Change tracking | Background job to persist changes & append historical records |
| News | Announcement model | Simple CRUD w/ role-based publishing |
| Blogs | Link submissions | URL validation, soft moderation queue |
| UI Enhancements | Dark mode toggle | CSS variable theme switch via Stimulus controller |
| Performance | Caching strategy | Fragment caching for directory pages |

## Tech Stack
| Layer | Tooling |
|-------|---------|
| Framework | Rails 8 (API + full-stack capabilities) |
| Language | Ruby 3.x (implied by Rails 8 target) |
| Data | PostgreSQL (`pg` gem) |
| Assets | Propshaft + `cssbundling-rails` (Sass) + Import Maps |
| Frontend Enhancements | Bootstrap 5.3, Bootstrap Icons, Stimulus, Turbo |
| Background Infra (planned) | `solid_queue`, `solid_cache`, `solid_cable` (already included) |
| Deployment | Kamal (container-based) + Puma + Thruster optimization |
| Dev Tooling | Rubocop (Rails Omakase), Brakeman, Debug gem, Tidewave |

## Local Development Setup
Prerequisites: Ruby (matching Rails 8 requirements), PostgreSQL, Node/Yarn (or npm), bundler.

```bash
git clone <repo-url>
cd stack_collective
bundle install
bin/rails db:prepare  # creates and migrates (schema tracked)
yarn install || npm install
bin/dev               # starts Rails w/ Procfile.dev (puma + css watcher + turbo) if configured
```

If CSS does not build automatically:
```bash
yarn build:css
```

## Directory & Feature Data (Future)
Planned relational structure (indicative):
```
users(id, github_uid, name, cohort_code, current_employment_id, created_at, updated_at)
companies(id, name, created_at, updated_at)
employments(id, user_id, company_id, title, started_on, ended_on, created_at, updated_at)
news_items(id, title, body, published_at, author_id)
blog_posts(id, user_id, url, title, submitted_at, approved_at)
```
Employment history displayed by querying `employments` ordered by `started_on` descending; current employer resolved through `current_employment_id` shortcut or latest where `ended_on IS NULL`.

## Styling & UX Notes
- Custom layout enhancements scoped under `.welcome-landing` to avoid global bleed.
- Hover elevation and gradient CTA implemented with accessible color contrast considerations.
- Placeholder shimmer uses Bootstrap `placeholder-wave` for loading affordance.

## Accessibility (Initial Considerations)
- Landmarks: `<main>`, `<nav>`, `<footer>` present.
- Link purpose: Anchor links use descriptive text.
- Color contrast: Primary gradient chosen to meet WCAG AA for large text (verify once final palette finalized).
- Next steps: Add skip link, aria-current on active nav, improved focus outlines.

## Development Workflow Suggestions
| Concern | Recommendation |
|---------|---------------|
| Branching | Feature branches via `feat/*`, `chore/*`, `fix/*` |
| Code Quality | Run Rubocop + Brakeman before PR |
| Security | Restrict OmniAuth callback host; enforce state param |
| Performance | Add indexes early (cohort_code, employer relations) |
| Background Jobs | Use `solid_queue` for async ingestion tasks |

## Testing (Planned)
- Model specs: validations, association integrity, employment transitions
- Request/system specs: auth flow, directory filtering
- Background job specs: employment change persistence, news publish events

## Deployment (Future Path)
- Kamal container build pipeline
- Multi-stage Docker optimization (asset build + runtime layer)
- DB migrations executed during `deploy run` phase
- Health checks: `/up` endpoint (add later)

## Contributing Guidelines (Draft)
1. Open an issue describing change intent.
2. Create a descriptive branch name.
3. Ensure lint/security checks pass (`rubocop`, `brakeman`).
4. Provide concise PR description with screenshots for UI changes.
5. Keep commits focused and logically grouped.

## License
License not yet specified. Add SPDX identifier once decision made (e.g., MIT, Apache-2.0).

## Acknowledgments
Inspired by the collaborative spirit and engineering culture fostered by the former Turing School of Software & Design alumni network.

---
For future enhancement ideas see Roadmap section. Additional architectural or data model proposals welcome via issues.
