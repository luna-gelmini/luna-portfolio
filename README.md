# C'est la Luna — portfolio (ladyluh.dev)

Source of [ladyluh.dev](https://ladyluh.dev), a personal portfolio presented as a small desktop environment: a top bar with a clock and taskbar, tiled windows you can open, close and drag onto each other to swap places, and a particle background. Built with Angular 21 standalone components, signals and zoneless change detection, styled with Tailwind CSS and deployed on Vercel.

## Features

- **Window manager in the browser.** Window state lives in a signal-backed map. The grid layout is derived from how many windows are open (fixed presets for 1–9 windows, square-root fallback beyond that), the last row is span-balanced, and windows can be dragged onto each other to swap positions, with a ghost preview following the cursor. Below 768 px wide everything stacks in one column.
- **Apps**, each a standalone component in `src/components/apps/`:
  - `about` — short bio and a quote.
  - `projects` — fetches the author's public repositories live from the GitHub API (`/users/luna-gelmini/repos?sort=updated&per_page=12`), skips forks and shows language, stars and topics.
  - `fetch` — a neofetch-style card with ASCII art and static system info.
  - `skills` — an icon grid loaded from public SVG icon CDNs.
  - `contact` — mailto button and social links.
  - `snake` — a playable snake game on a 20×20 grid (arrow keys).
  - `htop` — a mock process list of the open windows; "KILL" closes the window.
  - `cmatrix` — canvas character rain.
- **Hidden poetry workspace.** Running `unlockPoetry()` in the browser console reveals a second workspace with a file-explorer window listing 33 poems (in Portuguese) that open in a notepad-style window.
- **Background.** A Three.js scene with 15,000 additive-blended points on a fixed canvas behind the UI, plus a CSS scanline overlay.
- **SEO and Open Graph** meta tags in `index.html`.

## Stack

| Technology | Role |
|---|---|
| Angular 21 (`@angular/core`, `@angular/build`) | Standalone components, signals, `provideZonelessChangeDetection()`, built-in control flow (`@if`, `@for`, `@switch`) |
| TypeScript ~5.8 | Application code |
| Tailwind CSS | Utility classes, loaded through the CDN script in `index.html` |
| Three.js r128 | Particle background, loaded from cdnjs as the global `THREE` |
| Fira Code (Google Fonts) | Monospace typography |
| GitHub REST API | Live project list |
| Vercel | Hosting and build (`vercel.json`) |

## Getting started

Prerequisites: Node.js and npm (no versions are pinned in the repository).

```
npm install --legacy-peer-deps   # same flag Vercel uses, see vercel.json
npm run dev                      # ng serve on http://localhost:3000
npm run build                    # production build to dist/
npm run preview                  # ng serve with the production configuration
```

The site needs internet access at runtime for Tailwind, Three.js, fonts, skill icons and the GitHub API.

## Project structure

```
index.html                     shell, CDN scripts, import map, global styles
index.tsx                      bootstrapApplication(AppComponent) with zoneless change detection
angular.json                   build and serve config (entry index.tsx, port 3000, output dist/)
vercel.json                    build and install commands, cache headers for static assets
src/app.component.*            window manager, drag/swap, grid layout, Three.js background
src/components/ui/             top bar and window tile
src/components/apps/           the individual "apps"
src/data/portfolio.data.ts     texts, skills, socials and poems
```

## Deployment

Vercel runs `npm install --legacy-peer-deps` and `npm run build`, then serves `dist/` with one-year cache headers for images, scripts, styles and fonts (`vercel.json`). Any static host that can run the Angular build works the same way.

## Notes and limitations

- The static `PROJECTS` array in `src/data/portfolio.data.ts` is not used by the UI; projects come from the GitHub API.
- Unauthenticated GitHub API requests are rate-limited per IP; when the limit is hit the projects window shows its error state.
- There are no automated tests.

## License

No license file is included. All rights reserved by default, including the poems.
