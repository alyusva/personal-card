# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
npm run dev       # Start dev server at localhost:4321
npm run build     # Production build to dist/
npm run preview   # Preview the production build locally
```

Both `npm` and `pnpm` lock files are present; prefer `npm` for consistency with `package-lock.json`.

There are no tests or linters configured in this project.

## Architecture

This is a single-page Astro portfolio site deployed at [alvaro-yuste.com](https://alvaro-yuste.com). The entire site is a single route (`src/pages/index.astro`) composed of section components stacked vertically.

**Component flow:**
```
index.astro
  └── BaseLayout.astro       (HTML shell, fixed nav, ThemeToggle)
        └── [slot]
              ├── Hero.astro
              ├── About.astro
              ├── Experience.astro
              ├── Education.astro
              ├── Projects.astro
              └── Contact.astro  (also serves as the <footer>)
```

**All content is hardcoded directly in the component files** — there is no CMS, data files, or external API. To update profile content (jobs, projects, education), edit the relevant `.astro` component.

- `Projects.astro` defines projects as a local JS array in the frontmatter (`---` block), then maps over them to render cards.
- `Experience.astro` and `Education.astro` contain job/degree entries as plain HTML markup.
- `Hero.astro` and `About.astro` accept props passed from `index.astro` (name, position, cvLink, profileImage).

## Theming

Dark/light mode is implemented entirely via CSS custom properties. The `<html>` element gets a `data-theme="dark"|"light"` attribute toggled by `ThemeToggle.astro`. All color variables are defined in `src/styles/global.css` under `:root` (light) and `[data-theme="dark"]` (dark). The theme is persisted in `localStorage` and initialised inline (`is:inline` script) to avoid flash.

When adding new styles, use the existing CSS variables (`--color-text-primary`, `--color-accent`, `--space-md`, etc.) rather than hardcoded values so dark mode works automatically.

## Styling conventions

- Each component has a `<style>` block with **scoped CSS** — styles only apply to that component.
- To target the `[data-theme="dark"]` selector from inside a component, use `:global([data-theme="dark"]) .selector { }`.
- Utility classes (`.container`, `.section`, `.glass`, `.title-large`, `.title-section`, `.text-gradient`) are defined in `global.css` and can be used in any component.
- Icons come from Bootstrap Icons loaded via CDN in `BaseLayout.astro` (`<i class="bi bi-*">`).
- Company/school logos are stored in `public/images/logos/` and served at `/images/logos/<file>`.
- The CV PDF is at `public/files/` and served at `/files/CV Alvaro Yuste Valles.pdf`.

## Deployment

The site is deployed on Netlify. Vercel Analytics (`@vercel/analytics/astro`) is injected in `index.astro` via the `<Analytics />` component.
