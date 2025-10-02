# Tic Tac Toe — Svelte (Ocean Professional)

A minimalist, responsive, two‑player Tic Tac Toe built with SvelteKit. Features a modern “Ocean Professional” theme with blue and amber accents, smooth transitions, and clear on‑screen indicators for turns, wins, and draws.

## Quick start

Install dependencies and run the dev server:

```bash
npm install
npm run dev
```

Build for production:

```bash
npm run build
npm run preview
```

## Features
- Two‑player local play (X and O alternate on the same device)
- Winner and draw detection with visual highlights
- Reset button to start a new game
- Responsive, centered layout (mobile and desktop)
- Modern theme with rounded corners, subtle shadows, and gradient accents
- Smooth CSS transitions for interactions and states

## Project structure
- src/app.css — Global Ocean Professional theme variables and utilities
- src/routes/+layout.svelte — Global layout shell
- src/routes/+page.svelte — Main Tic Tac Toe game UI and logic

No environment variables are required.
