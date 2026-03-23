# Absoludation Pitch Outline (DRAFT!!!)

## 1) Problem
Users jump across many apps/sites to find quality recommendations for books, cars, restaurants, and recipes. Discovery is fragmented and inconsistent.

## 2) Solution
Absoludation is a unified recommendation platform with a minimal, rounded dashboard UI and one search entry point:
- “Search cars, books, restaurants, recipes…”
- Books-first implementation now live
- Scalable architecture for additional categories

## 3) Current Product Scope (MVP)
### Implemented
- Homepage with search, category pills, and right-side slim navigation
- Books recommendation flow (default + genre-filtered suggestions)
- Search results page with normalized book cards
- Book preview page with metadata, description, rating widget, comments, and preview/open flow
- Theme toggle (light/dark), responsive layout, accessibility-oriented controls

### Routes
- `/` Home
- `/search` Search Results
- `/book/:bookId` Book Preview

## 4) Key User Experience
- Fast discovery from one input
- Clear card-based content hierarchy
- Compact, dashboard-like layout
- Mobile-friendly and keyboard-accessible interaction

## 5) Technical Approach
- React + Vite frontend
- React Router for page flow
- Service-layer API integration and normalization (`src/services/googleBooks.js`)
- Reusable components (`SearchBar`, `CategoryPills`, `BookCard`, `Layout`)
- Extensible domain pattern for future categories

## 6) Why This Works
- Starts with one domain (Books) to validate UX and architecture
- Reuses same data-contract and UI patterns for Cars/Restaurants/Recipes
- Minimizes rewrite risk by separating UI components from API-specific parsing

## 7) Expansion Plan
### Cars
- Add car API service + filters (make/model/year/body/fuel)
- Compare/save workflow

### Restaurants
- Add location-aware API + filters (cuisine/rating/price/distance)
- Open-now and dietary preference tags

### Recipes
- Add recipe API + filters (diet/allergens/prep time)
- Ingredient-based recommendations and meal-planning hooks

## 8) Success Metrics
- Search-to-click-through rate
- Time-to-first-relevant-result
- Return sessions/bookmarks saved
- Category expansion adoption (non-book usage)

## 9) Next Milestones
1. Stabilize Books MVP polish + QA
2. Launch Restaurants module as next vertical
3. Add Recipes, then Cars
4. Introduce user profiles and cross-category personalization
