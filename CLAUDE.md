# CLAUDE.md

This file provides guidance for AI assistants (Claude Code and others) working in this repository.

## Repository Overview

This is the **GitHub profile repository** for `vdhvu` (Vo Duc Hoang Vu). When a repository's name matches its owner's GitHub username, GitHub automatically renders `README.md` as the public-facing profile page visible at `https://github.com/vdhvu`.

The primary deliverable is `README.md`. This file is the only content most visitors will ever see, so quality, clarity, and visual polish matter.

## Repository Structure

```
voduchoangvu/
├── CLAUDE.md          # This file — AI assistant guidance
└── README.md          # GitHub profile page (the main deliverable)
```

Additional assets (images, badges, stats widgets) may be added over time.

## Development Workflow

### Branching

- **Default branch**: `main` (or `master` — whichever GitHub sets on first push)
- Feature work and AI-assisted updates go on topic branches (e.g. `claude/...`)
- Merge to `main`/`master` to publish changes to the live profile page

### Making Changes

1. Edit `README.md` directly — no build step required.
2. Preview locally with any Markdown renderer (VS Code preview, `grip`, GitHub CLI `gh repo view --web`, etc.).
3. Commit with a descriptive message, then push.
4. The profile page updates immediately after the commit lands on the default branch.

### Commit Style

Use short, imperative commit messages:

```
Update README headline and bio
Add GitHub stats badges
Refresh skills section with 2025 stack
```

No ticket numbers or lengthy footers are needed for a profile repository.

## README Conventions

### Content Sections (suggested order)

1. **Headline / greeting** — one punchy line that sets the tone
2. **About me** — 2–4 sentences: role, interests, location (optional)
3. **Tech stack / skills** — badges or a concise list; keep it current
4. **Featured projects** — 2–4 pinned or highlighted repos with one-line descriptions
5. **Stats / activity** — GitHub stats cards, streak widget, top languages (optional)
6. **Contact / social links** — email, LinkedIn, portfolio, etc.

### Markdown Style

- Use ATX headings (`##`, `###`) — avoid `===`/`---` underline style.
- Prefer reference-style links for repeated URLs to keep source readable.
- Keep line lengths reasonable (≤ 120 chars) but don't hard-wrap inside sentences.
- Avoid raw HTML unless necessary for alignment or image sizing.
- Alt text on every image for accessibility.

### Badges

Use [shields.io](https://shields.io) or pre-made badge packs (e.g. `simple-icons`). Keep the palette consistent — pick one style (`flat`, `flat-square`, or `for-the-badge`) and stick with it throughout.

### GitHub Stats Widgets

Common widgets pulled via `readme-stats` or similar services:

```md
![GitHub Stats](https://github-readme-stats.vercel.app/api?username=vdhvu&show_icons=true&theme=...)
![Top Languages](https://github-readme-stats.vercel.app/api/top-langs/?username=vdhvu&layout=compact&theme=...)
![Streak](https://streak-stats.demolab.com?user=vdhvu&theme=...)
```

Replace `theme=...` with a consistent theme name. These widgets are fetched at render time by GitHub — no caching step needed.

## Key Constraints

- **No build pipeline** — everything is static Markdown; avoid adding unnecessary tooling.
- **No secrets** — never commit API keys, tokens, or personal identifiers beyond what is already public on the profile.
- **Keep it concise** — profile READMEs lose impact when they become walls of text. Favour visuals and bullet points over paragraphs.
- **Mobile rendering** — GitHub renders profiles on mobile; test wide tables and large image grids carefully.

## Testing / Validation

There is no test suite. Quality checks are manual:

1. Render the Markdown locally before pushing (`grip README.md` or VS Code preview).
2. After pushing to the default branch, visit `https://github.com/vdhvu` in a browser to confirm it looks correct.
3. Check both light and dark GitHub themes if using colour-sensitive badges.

## AI Assistant Guidelines

- When updating `README.md`, preserve the existing tone and style unless the user explicitly asks for a rewrite.
- Do not add placeholder text like `<!-- TODO -->` or `[Your name here]` — fill in real content or ask the user.
- When adding new badges or widgets, match the style (size/shape) already in use.
- Do not commit or push unless the user confirms the content looks correct, since the profile page is immediately public.
- Prefer editing `README.md` in place over creating additional Markdown files.
