# Pokémon Explorer — React Pokédex for Generation I

A polished, animated Pokédex-style web app that browses the first **150 Pokémon** using the **PokéAPI**, with fast client-side filtering, responsive layout, and motion-driven micro-interactions.

- **Live Demo:** https://pokemon-app-kappa-eight.vercel.app/  
- **Repository:** https://github.com/RiyaKaushik-tech/pokemon_app

---

## Overview

**Pokémon Explorer** is a single-page React application built with a modern Vite toolchain and a utility-first styling system. It fetches Pokémon data from the public PokéAPI, renders a responsive card grid, and provides a search + type filter experience with animated UI elements and an expandable modal for detail viewing.

The project is intentionally lightweight: no backend, no authentication, and no external state libraries—just React state, component composition, and disciplined UI logic.

---

## Key Features

- **Generation I catalog (first 150 Pokémon)** fetched from PokéAPI
- **Client-side search** (case-insensitive substring match)
- **Type-based filtering** (matches if any Pokémon type equals the selected type)
- **Responsive grid layout** (adapts from single-column to multi-column layouts)
- **Expandable Pokémon cards** with a modal for details (height, weight, ID, types)
- **Motion-first UX** using Framer Motion (grid entrance, card hover/tap, modal transitions)
- **Animated ambient background** (floating pastel “bubble” particles)
- **Inline “Fun facts” system** with per-Pokémon facts and a fallback fact

---

## Technical Architecture Overview

This repository is a **frontend-only SPA**.

### Data Flow
1. On initial load, `App.jsx` requests:
   - `GET https://pokeapi.co/api/v2/pokemon?limit=150`
2. For each returned Pokémon, the app fetches detailed data via `pokemon.url`.
3. The full dataset is stored in React state and becomes the source of truth for:
   - Search filtering
   - Type filtering
   - Suggestions list (names)

### UI Composition
- `App.jsx` orchestrates:
  - Global data fetching & error/loading states
  - Filtering logic (`useEffect` derived view state)
  - Grid rendering and entry animations
- `PokemonCard.jsx` handles:
  - Card UI + hover/tap animation
  - Modal open/close and modal animation
  - Displaying per-Pokémon facts via `src/data/pokemonFacts.js`
- `BubbleBackground.jsx` renders the animated ambient background layer.

---

## Tech Stack

| Category | Technology |
|---|---|
| **Frontend** | React 19, Vite |
| **Backend** | _None_ |
| **State Management** | React `useState`, `useEffect` (no external store) |
| **APIs** | PokéAPI (`https://pokeapi.co/api/v2`) |
| **Authentication** | _None_ |
| **Styling** | Tailwind CSS v4, DaisyUI |
| **Animation** | Framer Motion |
| **Tooling** | ESLint, Vite dev server/build/preview, npm |

---

## Folder Structure

```text
pokemon_app/
├─ public/
│  ├─ pokemmon_dektop.jpeg
│  ├─ pokemon.jpg
│  ├─ pokemon_mobile.jpg
│  └─ vite.svg
├─ src/
│  ├─ App.css
│  ├─ App.jsx
│  ├─ index.css
│  ├─ main.jsx
│  ├─ components/
│  │  ├─ BubbleBackground.jsx
│  │  ├─ Header.jsx
│  │  ├─ PokemonCard.jsx
│  │  └─ SearchBar.jsx
│  ├─ data/
│  │  └─ pokemonFacts.js
│  └─ assets/
├─ eslint.config.js
├─ index.html
├─ package-lock.json
├─ package.json
├─ tailwind.config.js
└─ vite.config.js
```

> Note: `src/assets/` exists but does not currently contain files in the retrieved listing.

---

## Installation & Setup

### Prerequisites
- Node.js (recommended: current LTS)
- npm (this repo includes `package-lock.json`)

### Steps
```bash
git clone https://github.com/RiyaKaushik-tech/pokemon_app.git
cd pokemon_app
npm install
npm run dev
```

Then open:
- http://localhost:5173

### Production build
```bash
npm run build
npm run preview
```

---

## Environment Variables

No `.env` / `.env.example` files or runtime environment variable references were detected in the codebase.  
All configuration is currently hard-coded (e.g., the PokéAPI base URL in `src/App.jsx`).

---

## Usage Guide

- **Browse:** Scroll through the grid of Pokémon cards.
- **Search:** Use the search input to filter Pokémon by name.
- **Filter by type:** Use the type dropdown to narrow results.
- **View details:** Click a card to open the modal with:
  - sprite image
  - name, ID
  - types (pill badges)
  - height and weight
  - a fun fact

---

## Engineering Highlights

- **Concurrent detail fetching:** After the initial list call, detailed Pokémon payloads are fetched in parallel via `Promise.all(...)` for faster aggregation.
- **Derived filtering state:** Filtering is computed from `pokemonList` + `search` + `selectedType` in a dedicated `useEffect`, keeping rendering logic predictable.
- **Motion system consistency:** Framer Motion is applied across:
  - header entrance
  - grid fade-in
  - per-card enter/hover/tap
  - modal open/close transitions
- **Fact lookup with safe fallback:** Facts are keyed by Pokémon name in `src/data/pokemonFacts.js`; missing entries resolve to `fallbackFact`.

---

## Performance & Optimization Notes

- **Network cost:** Fetching 150 Pokémon and then fetching each detail results in **151 requests** on first load. This is acceptable for a demo but can be optimized (see Future Improvements).
- **Viewport-based animations:** Cards use `whileInView` with `viewport: { once: true }` to avoid replaying entrance animations repeatedly during scrolling.

---

## Security Considerations

- No authentication or sensitive data handling is implemented.
- The app calls a public API directly from the client; ensure typical frontend hardening if extending this project:
  - handle API rate limits and transient failures gracefully
  - avoid trusting API payloads implicitly for future HTML injection scenarios (currently rendered as plain text)

---

## Future Improvements

- Reduce first-load request volume (batching strategy, caching, or progressive hydration of details)
- Normalize type filtering values (current UI uses capitalized labels while the API uses lowercase type names)
- Add request cancellation to prevent state updates on unmounted components during slow networks
- Improve accessibility (focus trapping in modal, ESC-to-close, ARIA labels)
- Add tests (component tests for filtering and modal behavior)

---

## Author

**Riya Kaushik**

---
