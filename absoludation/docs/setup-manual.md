# Draft Manual Setup & Run Guide (Not Yet Committed)

This project is a **draft React implementation** of a recommendation homepage focused on **Books**.

## 1) Prerequisites
- Node.js 18+ recommended
- npm installed

## 2) Install dependencies
From repository root:

```bash
cd absoludation/web
npm install
```

## 3) Run development server
```bash
npm run dev
```

Open the local URL shown in terminal (usually `http://localhost:5173`).

## 4) Build for production test
```bash
npm run build
npm run preview
```

## 5) What this draft includes

### Landing + Layout
- Rounded, minimal page shell with right-side slim vertical navigation
- Search bar placeholder:
  - `Search cars, books, restaurants, recipes...`
- Category widgets:
  - Cars, Books, Restaurants, Recipes

### Richer visual homepage
- Sectioned card-based layout (hero + trending section)
- Decorative hero graphic and quick stat chips
- Homepage is populated immediately with trending/popular-like books (randomized curated queries)

### Books recommendations behavior
- Default recommendations on first load:
  - Curated from multiple metadata queries (bestseller/popular/award-winning themes)
- Genre filter chips:
  - All, Fiction, Fantasy, Romance, History, Sci-Fi, Mystery
- For genre filtering, books are fetched by query and category-matched when possible

### Search and results
- Routing:
  - `/` -> Homepage
  - `/search?q=<query>&category=books` -> Search Results
- Search results show:
  - Title, author(s), published year, ISBN, cover image, details link
- If `/search` has no query, trending recommendations are shown by default
- Includes loading/error/empty/success handling

### Card style
- Squared, compact, info-first cards inspired by section/card dashboards
- Emphasizes title, author, and year; ISBN + details link as secondary metadata

### Accessibility + responsiveness
- Keyboard focus states for interactive controls
- Semantic sections/labels and live region for results messaging
- Responsive adaptation for tablet/mobile layouts

## 6) Data source
- Uses Open Library Search metadata API:
  - `https://openlibrary.org/search.json?q=<query>`
- Cover images are loaded from Open Library Covers API:
  - `https://covers.openlibrary.org/b/id/<cover_id>-M.jpg`

## 7) Notes
- Cars/restaurants/recipes remain placeholders for future scaling.
- This is intentionally **draft code** and **not committed** to GitHub.

## 8) Full Team/Student Implementation Manual
For complete architecture, phased implementation, service-layer strategy, testing workflow, and extension plans for **Cars / Restaurants / Recipes**, read:

- `../docs/IMPLEMENTATION_MANUAL.md`
