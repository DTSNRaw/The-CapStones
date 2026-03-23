# STEERED Design Style Guide

This style guide defines the visual and interaction standards for the Absoludation recommendation platform.

**Product scope today:** Books

**Product scope tomorrow:** Cars, Restaurants, Recipes (same design language)

---

## 1) Design Principles

1. **Minimal, not empty**  
   Keep interfaces clean, but always informative.

2. **Rounded and approachable**  
   Use soft corners across cards, buttons, inputs, and widgets.

3. **Information-first hierarchy**  
   Prioritize title → key metadata → secondary actions.

4. **Consistent rails and cards**  
   Maintain dashboard rhythm: content area + supportive right-rail widgets.

5. **Accessible by default**  
   Keyboard support, clear focus states, readable contrast, semantic structure.

---

## 2) Color System

Use semantic tokens rather than hardcoded colors in components.

## Core semantic tokens
- `--bg-main`: main page background
- `--bg-soft`: card/widget surfaces
- `--bg-elevated`: popout/highlight surfaces
- `--text-main`: primary text
- `--text-muted`: secondary metadata
- `--border-soft`: subtle separators
- `--accent`: primary action color
- `--accent-soft`: accent background for chips/buttons
- `--success`, `--warning`, `--error`: state colors
- `--focus-ring`: keyboard focus outline

## Theme support
- Implement via `data-theme="light|dark"` on root
- Keep semantic meaning stable across themes (only values change)
- Never rely on color alone to convey status

---

## 3) Typography

## Font families
- Primary UI: modern sans-serif stack (system-first)
- Optional mono only for technical values or code snippets

## Type scale
- Page title: 28–34px, medium/semibold
- Section title: 20–24px
- Card title: 16–18px
- Body text: 14–16px
- Metadata/captions: 12–14px
- Micro labels/tags: 11–12px

## Type behavior
- Keep line-height generous (1.4–1.6)
- Avoid all-caps for paragraph content
- Metadata should be visually quieter than title and description

---

## 4) Spacing, Layout, and Grid

## Spacing scale (recommended)
- 4, 8, 12, 16, 20, 24, 32, 40

## Layout rules
- Use consistent container padding
- Preserve vertical rhythm between stacked cards
- Avoid touching containers; maintain visible separation
- Keep right-side rail narrow and supportive

## Border radius
- Inputs/buttons/chips: 999px (pill) or 10–14px
- Cards/widgets: 14–18px
- Preview and large panels: 16–20px

---

## 5) Component Guidelines

## 5.1 Search Bar
- Placeholder text:
  - `Search cars, books, restaurants, recipes...`
- Rounded container, left icon, high readability
- Enter submits; search button optional but visible on desktop
- Strong focus ring on keyboard focus

## 5.2 Category Pills (Cars / Books / Restaurants / Recipes)
- Rounded pills with icon + label
- One active state clearly indicated
- Active state should not rely only on color (weight, border, or icon fill change too)

## 5.3 Right Vertical Navigation Rail
- Slim right rail with:
  - Home
  - Search
  - Discovery (compass)
  - Bookmarks
- Icon-first layout with clear hover/active/focus feedback
- Avoid large labels that clutter narrow space

## 5.4 Cards (BookCard and future domain cards)
- Info hierarchy:
  1) Title
  2) Subtitle/author
  3) Supporting metadata (year, ISBN, rating)
- Cover/image ratio should remain stable
- Primary action should be obvious (“Details”, “Preview”, etc.)

## 5.5 Widgets (Quick stats, ratings, quiz, etc.)
- Keep compact and supportive (not dominant)
- Right-side widgets should align visually with card system
- Use short labels and concise values

## 5.6 Buttons and Icon Buttons
- Rounded style, medium density
- Icon-only buttons must include accessible label (`aria-label`)
- Distinct states: default, hover, active, disabled, focus-visible

## 5.7 Forms and Inputs
- Clear labels where context is not obvious
- Error message appears near field and uses semantic error color + icon/text
- Keep helper text concise

---

## 6) Iconography and Imagery

- Use consistent icon set (stroke width/shape family aligned)
- Default icon size:
  - Navigation: 18–22px
  - Inline actions: 14–18px
- Decorative imagery should not compete with primary information
- Always provide meaningful alt text for informative images

---

## 7) Motion and Interaction

- Keep motion subtle and fast:
  - 120–220ms for hover/focus transitions
- Avoid excessive bounce/parallax
- Animate opacity/transform over layout-changing properties when possible
- Provide clear feedback for async states:
  - loading
  - success
  - error
  - empty

---

## 8) Accessibility Standards

- Meet WCAG contrast expectations (at least AA where feasible)
- All interactive elements keyboard reachable
- Visible `:focus-visible` states required
- Icon-only controls require `aria-label`
- Use semantic landmarks (`header`, `main`, `nav`, `section`)
- Announce dynamic list changes where needed (e.g., search results)

---

## 9) Responsive Behavior

## Recommended breakpoints
- Mobile: `< 640px`
- Tablet: `640–1024px`
- Desktop: `> 1024px`

## Behavior by size
- Collapse multi-column layouts to single-column on smaller widths
- Stack right-rail widgets below primary content on mobile
- Maintain readable tap targets (minimum ~40px)

---

## 10) Content and Microcopy

- Use plain, helpful language
- Keep labels short and action-oriented
- Prefer sentence case over title case for body/interface labels
- Error messages should explain what happened + what to do next

Example:
- Good: “Preview unavailable for this edition. Open source page instead.”
- Avoid: “Error occurred.”

---

## 11) Do / Don’t

## Do
- Keep metadata compact and secondary
- Keep spacing consistent between content blocks
- Reuse component patterns across categories

## Don’t
- Mix multiple corner-radius styles randomly
- Oversize preview iframes so they dominate page
- Depend on color alone for state communication
- Put API/parsing logic inside visual components

---

## 12) Cross-Category Consistency (Books, Cars, Restaurants, Recipes)

As new categories are added:

1. Reuse shared shell layout (search + pills + rail)
2. Keep card anatomy consistent (title/subtitle/meta/action)
3. Map provider responses to a normalized display model
4. Keep filtering UI and action patterns familiar across categories
5. Preserve token-based theming and spacing rules

---

## 13) Implementation Notes for Developers

- Prefer CSS custom properties/tokens over hardcoded values
- Keep component styles scoped and predictable
- Update this guide whenever:
  - new component families are introduced
  - color tokens or spacing scale changes
  - accessibility patterns are revised

This guide is the baseline for all current and future STEERED UI work.
