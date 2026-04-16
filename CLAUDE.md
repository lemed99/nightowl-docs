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
├── agent/              # Agent deployment + scaling
│   ├── migration-from-nightwatch.mdx
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

**12 MDX pages total** across 4 content groups.

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
- Cross-links via relative paths (e.g., `/agent/migration-from-nightwatch`)
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

## Adding a Navigation Group

Edit `docs.json` → `navigation.tabs[0].groups` — each group has `group` label + `pages` array (paths without `.mdx`).

## Deployment

No in-repo config. Mintlify watches the GitHub repo; pushes to `main` trigger a rebuild. Manual trigger: commit a no-op (see commit `3494422 chore: trigger docs redeployment`).

Domain is managed in the Mintlify dashboard, not this repo.
