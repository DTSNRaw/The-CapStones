# STEERED API Endpoints

This document defines the endpoint strategy for the STEERED recommendation platform.

- Current implementation status: **Books**
- Current architecture: **Frontend React app calls external data sources directly through a service layer**
- Internal backend: **Not implemented yet** (planned for future consolidation)

---

## 1) API Architecture Overview

## Current (Implemented)
- UI routes call functions in `web/src/services/googleBooks.js`
- Service functions fetch and normalize data from:
  - Open Library Search + metadata endpoints
  - Open Library Covers API
  - Archive/Open Library preview-compatible links (fallback flow)

## Planned (Future)
- Add backend/API gateway (recommended) to:
  - unify all category providers (Books/Cars/Restaurants/Recipes)
  - handle API keys securely server-side
  - cache responses and apply retry/rate-limit policies centrally

---

## 2) Implemented External Endpoints (Books)

> Note: The service filename is `googleBooks.js` for project continuity, but current data source behavior is Open Library/Archive-compatible.

## 2.1 Open Library Search
- **Method:** `GET`
- **URL:** `https://openlibrary.org/search.json`
- **Primary use:** Query books for homepage recommendations and search results

### Query Params
- `q` (required): search query
- Optional common params:
  - `limit`
  - `page`
  - `language`
  - `subject`

### Example
```bash
curl "https://openlibrary.org/search.json?q=best%20sellers&limit=20"
```

### Key fields typically used from response docs
- `key`
- `title`
- `author_name`
- `first_publish_year`
- `isbn`
- `cover_i`
- `language`
- `subject`

---

## 2.2 Open Library Work/Edition Metadata (Pattern)
- **Method:** `GET`
- **URL pattern:** `https://openlibrary.org{work_or_edition_key}.json`
- **Primary use:** enrich details for preview page (description/excerpts/metadata resolution when needed)

### Example
```bash
curl "https://openlibrary.org/works/OL45883W.json"
```

---

## 2.3 Open Library Covers API
- **Method:** `GET`
- **URL pattern:** `https://covers.openlibrary.org/b/id/{cover_id}-{size}.jpg`
- **Primary use:** cover image rendering

### Path Params
- `{cover_id}`: numeric cover id
- `{size}`: `S` | `M` | `L`

### Example
```bash
curl -I "https://covers.openlibrary.org/b/id/240727-M.jpg"
```

---

## 2.4 Preview/Read Fallback Links (Archive/Open Library-Compatible)
- **Method:** `GET` (link/embed usage)
- **Primary use:** render or link book previews from available identifiers
- **Behavior in service layer:**
  1. Try available preview/embed candidates
  2. Resolve archive-related identifiers if present
  3. Fall back to Open Library preview/read links
  4. Return `previewSource` metadata for UI messaging and fallback states

---

## 3) Normalized Book Data Contract (UI-facing)

The service layer should shape provider-specific responses into a stable UI contract:

```json
{
  "id": "string",
  "title": "string",
  "authors": ["string"],
  "year": 2020,
  "isbn": "string|null",
  "coverImage": "string|null",
  "description": "string|null",
  "excerpt": "string|null",
  "previewLink": "string|null",
  "previewEmbedUrl": "string|null",
  "hasPreview": true,
  "previewSource": "string|null",
  "rating": 4.2
}
```

---

## 4) Frontend Route → Endpoint Mapping

| Frontend Route | Purpose | Endpoint Patterns |
|---|---|---|
| `/` | Homepage recommendations/trending + genre chips | `GET /search.json?q=<curated_query>` |
| `/search?q=<query>&category=books` | Search result listing | `GET /search.json?q=<query>&limit=<n>` |
| `/book/:bookId` | Book detail + preview | `GET /works/{id}.json` or equivalent metadata resolution + preview fallback strategy |

---

## 5) Error Handling + Resilience Guidelines

- Handle states in UI:
  - `loading`
  - `empty`
  - `error`
  - `success`
- Use defensive parsing for optional fields (`isbn`, `cover_i`, description variants)
- If preview URL is unavailable:
  - show clear fallback message
  - provide external “Open preview/source” link where possible
- Log source/fallback path in development for debugging preview failures

---

## 6) Caching, Rate Limits, and Retry Guidance

## Client-side recommendations (current architecture)
- Debounce search input (e.g., 250–400ms)
- Cache recent queries in memory/session state
- Prevent duplicate concurrent request bursts for same query
- Use basic retry on transient network errors (1 retry max)

## Future backend recommendations
- Add short-lived cache (e.g., 5–15 minutes for popular queries)
- Standardized exponential backoff for provider failures
- Introduce quota/telemetry dashboards by provider

---

## 7) Security Guidance

Current books providers used here are generally keyless/public endpoints.  
For future providers requiring keys (Cars/Restaurants/Recipes):

- Never hardcode keys in frontend code
- Store keys in backend env vars
- Expose only backend endpoints to frontend
- Apply provider-specific request signing/key rotation policies

---

## 8) Planned Category Endpoint Strategy

The same service normalization pattern should be used for every category.

## 8.1 Cars (Planned)
### Suggested providers
- NHTSA APIs
- CarQuery
- Commercial vehicle data APIs (if richer specs/images needed)

### Example endpoint templates
- `GET /cars?make=<make>&model=<model>&year=<year>`
- `GET /cars/{id}`

### Normalized shape
```json
{
  "id": "string",
  "make": "string",
  "model": "string",
  "year": 2022,
  "bodyType": "SUV",
  "fuelType": "Hybrid",
  "transmission": "Automatic",
  "image": "string|null",
  "detailLink": "string|null"
}
```

---

## 8.2 Restaurants (Planned)
### Suggested providers
- Yelp Fusion
- Google Places
- Foursquare

### Example endpoint templates
- `GET /restaurants?location=<lat,lng>&cuisine=<type>&price=<1-4>&openNow=true`
- `GET /restaurants/{id}`

### Normalized shape
```json
{
  "id": "string",
  "name": "string",
  "cuisine": "string",
  "rating": 4.5,
  "priceLevel": 2,
  "address": "string",
  "distance": 1.2,
  "image": "string|null",
  "mapsLink": "string|null"
}
```

---

## 8.3 Recipes (Planned)
### Suggested providers
- Spoonacular
- Edamam
- TheMealDB

### Example endpoint templates
- `GET /recipes?query=<text>&diet=<diet>&exclude=<allergens>&maxReadyTime=<min>`
- `GET /recipes/{id}`

### Normalized shape
```json
{
  "id": "string",
  "title": "string",
  "cuisine": "string|null",
  "dietTags": ["vegan", "gluten-free"],
  "prepTime": 25,
  "calories": 420,
  "image": "string|null",
  "sourceLink": "string|null"
}
```

---

## 9) Recommended Unified Cross-Category Contract

To support shared UI components, map each category to a common card model:

```json
{
  "id": "string",
  "category": "books|cars|restaurants|recipes",
  "title": "string",
  "subtitle": "string",
  "meta": ["string"],
  "image": "string|null",
  "rating": 4.0,
  "tags": ["string"],
  "detailLink": "string|null"
}
```

---

## 10) Verification / Testing (CLI)

## Books search endpoint check
```bash
curl "https://openlibrary.org/search.json?q=science%20fiction&limit=5"
```

## Work metadata check (example)
```bash
curl "https://openlibrary.org/works/OL45883W.json"
```

## Cover availability check
```bash
curl -I "https://covers.openlibrary.org/b/id/240727-M.jpg"
```

## What to verify
- HTTP success codes (typically `200`)
- Response contains expected keys
- UI normalization handles missing optional fields gracefully
- Preview fallback behavior remains functional when direct preview URL is absent

---

## 11) Future Internal API (Recommended)

When backend is introduced, expose internal endpoints such as:

- `GET /api/recommendations?category=books&query=<q>`
- `GET /api/books/:id`
- `GET /api/cars`
- `GET /api/restaurants`
- `GET /api/recipes`

Frontend then calls only `/api/*`, while backend handles external provider integrations, keys, and caching.
