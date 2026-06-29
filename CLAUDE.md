# NightOwl Docs

## What This Is

Product documentation site for NightOwl. Pure **Mintlify** — no build step, no `package.json`. Content is MDX + JSON config. Deployed via Mintlify's hosted platform (not Vercel/Netlify).

Repo: `https://github.com/lemed99/nightowl-docs` (split from the monolith in Apr 2026).

## Stack

- **Mintlify** (configured via `docs.json`)
- MDX for content (Markdown + JSX components from Mintlify's library)
- SVG logos + favicon
- No Node dependencies, no scripts, no CI — push to `main` and Mintlify redeploys

## Structure

```
/
├── docs.json           # Mintlify config: theme, colors, navigation, anchors, navbar
├── favicon.svg
├── logo/
│   ├── light.svg
│   └── dark.svg
├── introduction.mdx    # Overview — what NightOwl is, how it works, key features
├── quickstart.mdx      # 5-step getting started guide
├── changelog.mdx       # Release notes (Mintlify <Update> entries, newest first)
├── agent/              # Agent deployment + scaling
│   ├── running-alongside-nightwatch.mdx
│   ├── multiple-instances.mdx
│   └── health-monitoring.mdx
├── dashboard/          # Feature guides
│   ├── issues.mdx
│   ├── alert-channels.mdx
│   ├── smtp-setup.mdx
│   ├── mcp-server.mdx
│   └── data-management.mdx
└── performance/        # Capacity planning
    ├── throughput.mdx
    └── postgresql-sizing.mdx
```

**13 MDX pages total** across 4 content groups plus a standalone changelog.

## `docs.json` — Mintlify Config

- Theme: `"mint"` (standard light/dark)
- Primary: `#34d399` (emerald, matches app/landing)
- Light accent: `#6ee7b7` / Dark: `#059669`
- Backgrounds: `#ffffff` (light) / `#0f0f0f` (dark)
- **Single anchor** ("Documentation", book icon) with 4 navigation groups:
  1. **Getting Started** — `introduction`, `quickstart`
  2. **Agent** — migration, multiple-instances, health-monitoring
  3. **NightOwl Features** — issues, alert-channels, smtp-setup, mcp-server, data-management
  4. **Performance** — throughput, postgresql-sizing
- Second anchor ("Changelog", rss icon) holding the single `changelog` page
- Global anchors (top-right): Agent README (GitHub), GitHub repo
- Navbar: "Dashboard" link + "Get Started" signup button

When adding a page, register it in `docs.json` under the appropriate navigation group.

## Content Style

Tone: **technical, product-first, action-oriented. No prose fluff.**

Every page:
- Frontmatter with `title` and `description` (used for meta tags)
- 1-2 sentence intro paragraph
- Numbered steps OR "The X signals" / "The Y types" sections
- Mintlify components: `<Card>`, `<CardGroup>`, `<Note>`, `<AccordionGroup>`, `<Accordion>`, `<Tabs>`, `<Tab>`
- Code blocks with explicit language (bash, env, json, php)
- Cross-links via relative paths (e.g., `/agent/running-alongside-nightwatch`)
- Status tables, configuration examples, diagnostic flowcharts

**Example page shape** (e.g., `dashboard/issues.mdx`):
1. Two-column `<CardGroup>` listing issue types
2. Status table with pipes (`open | resolved | ignored`)
3. Numbered triage workflow
4. Bulk actions list

**Don't**:
- Don't introduce a build step or add `package.json` — Mintlify eats MDX directly
- Don't embed screenshots inline unless genuinely useful (current content is illustration-free)
- Don't write long prose — customers scan, they don't read

## Adding a Page

1. Create `<group>/<slug>.mdx` with frontmatter `title` + `description`
2. Register the path in `docs.json` under the relevant group
3. Commit + push to `main` — Mintlify auto-rebuilds

## Maintaining the Changelog

`changelog.mdx` is the customer-facing release log. Conventions:

- Newest entries on top. Each release is one Mintlify `<Update label="Month D, YYYY" tags={[...]}>` block.
- `tags` are the affected surface(s): `Agent`, `API`, `Dashboard`, `Performance`, `Launch`.
- Group all repos' changes for a given date into one entry — customers don't care which repo shipped what.
- Write in customer language, not commit-speak. Translate "fix int4 overflow on duration columns" into the user-visible symptom and fix.
- Only list user-facing changes. Skip internal refactors, test-only commits, and CI tweaks.
- Every `Agent`-tagged entry ends with an **"Upgrading the agent:"** line that states the commands to run (`composer update`, `nightowl:migrate`, restart) and deep-links to the matching version section of the agent changelog: `https://github.com/lemed99/nightowl-agent/blob/main/CHANGELOG.md`. GitHub anchors strip brackets/dots, so `## [1.1.0] - 2026-06-04` → `#110---2026-06-04`. Source the exact upgrade steps from that repo's `CHANGELOG.md`, not from the commit log.
- Source of truth for new entries: `git log` across the five repos since the last changelog date.

## Adding a Navigation Group

Edit `docs.json` → `navigation.tabs[0].groups` — each group has `group` label + `pages` array (paths without `.mdx`).

## Deployment

No in-repo config. Mintlify watches the GitHub repo; pushes to `main` trigger a rebuild. Manual trigger: commit a no-op (see commit `3494422 chore: trigger docs redeployment`).

Domain is managed in the Mintlify dashboard, not this repo.
