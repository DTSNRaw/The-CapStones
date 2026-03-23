# STEERED Recommendation Platform — Implementation Manual

This manual explains how to build the current **recommendation web app (books only for now)** and how to extend it to **Cars, Restaurants, and Recipes** in a clean, scalable way.

---

## 1) Project Purpose

Build a minimal, friendly recommendation platform with:

- A rounded dashboard-like homepage
- A unified search bar:  
  `Search cars, books, restaurants, recipes...`
- Category chips/icons (Cars, Books, Restaurants, Recipes)
- Right-side slim vertical navigation (Home, Search, Discovery, Bookmarks)
- Books search and preview flow powered by API data
- Responsive + accessible UI
- Scalable architecture for additional categories

Current production focus is **Books**, with other domains designed as extension paths.

---

## 2) Tech Stack

- **Frontend:** React + Vite
- **Routing:** React Router
- **Styling:** CSS (custom, rounded dashboard system)
- **Data layer:** service-based API fetch modules
- **Current book data provider:** Open Library/Archive-compatible endpoints (service file currently named `googleBooks.js` for project continuity)

---

## 3) Current Project Structure

```txt
absoludation/
  web/
    src/
      components/
        Layout.jsx
        SearchBar.jsx
        CategoryPills.jsx
        BookCard.jsx
      pages/
        HomePage.jsx
        SearchResultsPage.jsx
        BookPreviewPage.jsx
      services/
        googleBooks.js
      App.jsx
      main.jsx
      App.css
      index.css
```

### Responsibilities by layer

- **`main.jsx`**: app bootstrap
- **`App.jsx`**: route map + theme state wiring
- **`components/`**: reusable UI blocks
- **`pages/`**: page-specific composition and interaction logic
- **`services/`**: API normalization, fallback logic, data shaping
- **`App.css` / `index.css`**: design system + page styling + dark mode

---

## 4) End-to-End Build Process (How to Recreate)

## Phase A — Scaffold + Base Setup

1. Scaffold with Vite React
2. Install dependencies
3. Create app shell with rounded card layout and right-side nav
4. Set up `App.jsx` routes:
   - `/`
   - `/search`
   - `/book/:bookId`

---

## Phase B — Homepage

Create `HomePage.jsx` with:

- Hero section
- Global search bar
- Category pills
- Genre filtering for books
- Recommended cards
- Quick widget rail (streak, top category, saved books)
- One-question-at-a-time mini preference quiz with progress bar and icon controls

---

## Phase C — Search Results

Create `SearchResultsPage.jsx` with:

- Query parsing via URL params
- Search execution through service layer
- Loading / success / empty / error states
- Card grid output with links to preview page

---

## Phase D — Book Preview Experience

Create `BookPreviewPage.jsx` with:

- Book metadata + cover + excerpt + description
- Preview widget with source/fallback behavior
- Comments section
- User star rating widget
- Cleaner dashboard spacing and section segmentation

Key UI refinements implemented:

- Rating widget aligned to right-side layout style
- Metadata moved above description
- Eye-icon preview trigger
- Reduced visual footprint for preview panel
- Added spacing to prevent card crowding

---

## Phase E — Theming + Accessibility

- Theme toggle (icon-only sun/moon)
- Persist `data-theme` in local storage
- Ensure all controls have focus-visible styles
- Use semantic sections/labels and ARIA for icon buttons and progress/rating controls

---

## 5) Books Service Layer Design (`src/services/googleBooks.js`)

Even though filename says `googleBooks.js`, treat it as a **books domain service abstraction**.

## Core responsibilities

1. Query + fetch books
2. Normalize result shape for UI components
3. Resolve author names (avoid raw `/authors/...`)
4. Resolve preview URLs from multiple signals
5. Fallback safely if preview is unavailable

## Typical normalized book shape

```js
{
  id,
  title,
  authors,
  year,
  isbn,
  coverImage,
  previewLink,
  previewEmbedUrl,
  hasPreview,
  description,
  excerpt,
  rating
}
```

## Preview strategy used

- Try archive identifiers (`ocaid`, `identifiers.ia`, links)
- Build possible embed URL
- Fallback to Open Library preview route
- Return preview source metadata for UI state control

---

## 6) Routing + Component Integration Pattern

Use this pattern for every domain:

1. **Page reads URL params**
2. **Page calls service**
3. **Service normalizes API output**
4. **Page renders normalized cards/widgets**
5. **Navigation preserves query context where useful**

This keeps components simple and API-specific logic centralized.

---

## 7) UI/UX Rules to Keep Consistent

- Rounded corners everywhere (pills/cards/panels/buttons)
- Clear card spacing (avoid touching containers)
- Compact metadata text for readability hierarchy
- Right rail widgets visually consistent
- Avoid oversized embeds that dominate layout
- Use icon controls where meaning is clear (with ARIA labels)

---

## 8) Testing Workflow for Teams

## Minimum CI/CLI checks

```bash
cd absoludation/web
npm run build
```

## Manual critical-path checks

1. Home page loads with recommendations
2. Search works and renders cards
3. Opening a book routes to preview page
4. Preview open/fallback behavior works
5. Comments can be posted locally
6. Star rating updates
7. Theme toggle updates UI
8. Responsive checks at desktop/tablet/mobile

---

## 9) Common Pitfalls + Fixes

1. **Preview not showing**
   - Ensure fallback path is displayed with link-out option
2. **Author shows as raw key**
   - Confirm author resolution helper/caching is used
3. **Layout cluttered**
   - Increase section margins/gaps and reduce metadata size
4. **Theme mismatch**
   - Use `:root[data-theme='dark']` consistently
5. **Search state confusion**
   - Keep query in URL and derive page state from URL first

---

## 10) How to Extend to Cars, Restaurants, Recipes

Use the **same architecture**:
- `pages/<Domain>SearchPage.jsx`
- `components/<Domain>Card.jsx`
- `services/<domainService>.js`
- shared search shell + domain switch handling

## A) Cars Recommendations

### Suggested APIs
- Car specifications datasets/APIs (e.g., NHTSA, CarQuery, RapidAPI providers)

### Data shape target
```js
{
  id,
  make,
  model,
  year,
  bodyType,
  fuelType,
  transmission,
  image,
  detailLink
}
```

### Features to implement
- Filter by make/year/body/fuel
- Compare 2–3 cars
- Save/bookmark options

---

## B) Restaurants Recommendations

### Suggested APIs
- Yelp Fusion API, Google Places, Foursquare, or similar location providers

### Data shape target
```js
{
  id,
  name,
  cuisine,
  rating,
  priceLevel,
  address,
  distance,
  image,
  mapsLink
}
```

### Features to implement
- Filter by cuisine, rating, distance, price
- Geo-aware recommendations
- “Open now” and dietary tags

---

## C) Recipes Recommendations

### Suggested APIs
- Spoonacular, Edamam, TheMealDB

### Data shape target
```js
{
  id,
  title,
  cuisine,
  dietTags,
  prepTime,
  calories,
  image,
  sourceLink
}
```

### Features to implement
- Filters by diet/allergens/prep time
- Ingredient-based search
- Save meal plans/bookmarks

---

## 11) Multi-Domain Design Recommendation

Create a shared interface contract per recommendation item:

```js
{
  id,
  title,        // generic primary label
  subtitle,     // secondary line
  meta,         // compact details
  image,
  rating,
  tags,
  detailLink,
  category      // books | cars | restaurants | recipes
}
```

Then map each domain-specific payload into this unified view model.

This allows:
- Shared cards
- Shared list/grid layouts
- Shared bookmark logic
- Easier analytics

---

## 12) Suggested Team Sprint Breakdown (Example)

- **Sprint 1:** Books baseline (search/results/preview)
- **Sprint 2:** Accessibility, theme, responsiveness, polish
- **Sprint 3:** Add Restaurants domain
- **Sprint 4:** Add Recipes domain
- **Sprint 5:** Add Cars domain + compare feature
- **Sprint 6:** Cross-domain recommendation blending + bookmarks backend

---

## 13) Code Review Checklist (for Team (US!))

- [ ] No API parsing logic in UI components
- [ ] Service layer returns normalized fields
- [ ] URL-driven state for search and detail pages
- [ ] Loading/error/empty states exist
- [ ] Focus-visible styles present on interactive controls
- [ ] Mobile layout verified
- [ ] No hardcoded secrets in frontend
- [ ] Build passes locally

---

## 14) Final Notes

This project is intentionally modular so our team can:
1) keep the polished dashboard UI,
2) plug in additional recommendation domains,
3) avoid major refactors when scaling.

Start with one domain done well (Books), then copy the same shit for Cars/Restaurants/Recipes through service normalization and shared components.
