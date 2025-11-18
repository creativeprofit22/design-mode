# 5 Design Quality Principles

**Purpose:** Mandatory quality enforcement framework for Design Mode (equivalent to "5 Critical Lessons" in Supah Copy Mazta)

**Status:** MANDATORY for all design output

---

## Why These Principles Exist

After initial implementation and testing, 5 recurring quality issues were identified that prevented designs from being production-ready, accessible, and performant. These principles prevent those issues.

---

## PRINCIPLE 1: COMPOSE, Don't Copy-Paste

**Problem:** Agents were recreating components from scratch instead of using Shadcn UI primitives.

**Solution:** Always build FROM Shadcn UI components, never reinvent.

---

### ✅ CORRECT Application

```tsx
// ✅ GOOD: Compose from Shadcn UI primitives
import { Button } from "@/components/ui/button"
import { Card, CardHeader, CardTitle, CardContent } from "@/components/ui/card"
import { Check } from "lucide-react"

export function PricingCard({ tier, price, features }) {
  return (
    <Card>
      <CardHeader>
        <CardTitle>{tier}</CardTitle>
      </CardHeader>
      <CardContent>
        <ul>
          {features.map(feature => (
            <li key={feature}>
              <Check className="h-4 w-4" />
              <span>{feature}</span>
            </li>
          ))}
        </ul>
        <Button>Subscribe</Button>
      </CardContent>
    </Card>
  )
}
```

**Why:** Shadcn UI components have:
- Built-in accessibility (ARIA, keyboard navigation)
- Dark mode support
- Consistent styling
- Battle-tested code (~32KB total vs. 120KB Material UI)

---

### ❌ WRONG Application

```tsx
// ❌ BAD: Recreating from scratch
export function PricingCard({ tier, price, features }) {
  return (
    <div className="rounded-lg border bg-white p-6">
      <h3 className="text-2xl font-bold">{tier}</h3>
      <div className="space-y-2">
        {features.map(feature => (
          <div key={feature}>
            <svg>{/* Custom checkmark */}</svg>
            <span>{feature}</span>
          </div>
        ))}
      </div>
      <button className="rounded-md bg-blue-600 px-4 py-2">
        Subscribe
      </button>
    </div>
  )
}
```

**Problems:**
- No accessibility (missing ARIA, keyboard navigation)
- No dark mode support
- Reinventing Button (Shadcn UI's Button handles focus states, disabled states, variants)
- Bigger bundle (custom code vs. reusable primitives)

---

### Test

**Before shipping a component, ask:**
> "Could I build this using Shadcn UI primitives instead of custom divs/spans?"

If yes → Use Shadcn UI primitives.

---

## PRINCIPLE 2: ACCESSIBILITY FIRST (Never Compromise WCAG AA)

**Problem:** Designs were visually appealing but failed WCAG 2.2 AA standards.

**Solution:** Accessibility is NON-NEGOTIABLE. Never ship inaccessible designs.

---

### ✅ CORRECT Application

```tsx
// ✅ GOOD: Accessible component
<Button
  className="
    bg-blue-600
    text-white
    focus:outline-none
    focus-visible:ring-2
    focus-visible:ring-blue-600
    focus-visible:ring-offset-2
  "
  aria-label="Subscribe to Pro plan"
>
  Subscribe
</Button>

// Color contrast: 4.8:1 (PASS)
// Focus indicator: Visible (PASS)
// Semantic element: <button> (PASS)
// Accessible name: aria-label (PASS)
```

---

### ❌ WRONG Application

```tsx
// ❌ BAD: Multiple accessibility failures
<div
  className="bg-blue-400 text-white cursor-pointer"
  onClick={handleClick}
>
  Subscribe
</div>

// Problems:
// ✗ Not keyboard accessible (not <button>)
// ✗ No focus indicator
// ✗ Color contrast: 2.9:1 (FAIL - needs 4.5:1)
// ✗ No accessible name for screen readers
```

---

### Mandatory Checks

Every component MUST pass:

1. **Color Contrast:**
   - Text: ≥ 4.5:1 (normal), ≥ 3:1 (large)
   - UI components: ≥ 3:1

2. **Keyboard Navigation:**
   - All interactive elements focusable (Tab key)
   - Focus indicators visible (2px ring minimum)
   - Logical tab order

3. **Semantic HTML:**
   - Use `<button>` not `<div role="button">`
   - Use `<nav>`, `<main>`, `<article>`, `<section>`
   - Proper heading hierarchy (h1 → h2 → h3)

4. **Screen Reader Support:**
   - All images have alt text (or alt="" if decorative)
   - All form inputs have labels
   - All icon-only buttons have aria-label

---

### Test

**Before shipping, verify:**
> "Does this component pass WCAG 2.2 AA standards in BOTH light and dark mode?"

If no → Fix before shipping.

---

## PRINCIPLE 3: PERFORMANCE BUDGET AWARENESS (Every Import Costs)

**Problem:** Agents were importing entire libraries, causing bloated bundles.

**Solution:** Be conscious of every import decision. Track bundle size.

---

### ✅ CORRECT Application

```tsx
// ✅ GOOD: Named imports (tree-shakeable)
import { Button } from '@/components/ui/button'
import { Check, ArrowRight } from 'lucide-react'

// Bundler includes ONLY:
// - Button component (~2KB)
// - Check icon (~0.5KB)
// - ArrowRight icon (~0.5KB)
// Total: ~3KB

// ✅ GOOD: Dynamic import for below-fold
import dynamic from 'next/dynamic'

const Pricing = dynamic(() => import('@/components/Pricing'))
const Testimonials = dynamic(() => import('@/components/Testimonials'))

// Loaded only when needed (not in initial bundle)
```

---

### ❌ WRONG Application

```tsx
// ❌ BAD: Wildcard imports (kills tree-shaking)
import * as UI from '@/components/ui'
import * as Icons from 'lucide-react'

// Bundler includes EVERYTHING:
// - All UI components (~50KB)
// - All 200+ icons (~100KB)
// Total: ~150KB (50x bloat!)

// ❌ BAD: Heavy library for simple task
import _ from 'lodash'
const sorted = _.sortBy(items, 'name')

// Imports entire lodash (~70KB)

// ✅ BETTER: Built-in method
const sorted = items.sort((a, b) => a.name.localeCompare(b.name))
// 0KB cost
```

---

### Performance Targets

**MVP (Landing Page):**
- Total JS: < 90KB gzipped
- LCP: < 2.0s
- CLS: < 0.05

**Phase 2 (With Animations):**
- Total JS: < 130KB gzipped
- LCP: < 2.5s
- CLS: < 0.1

---

### Test

**Before adding an import, ask:**
> "What's the bundle cost? Is there a lighter alternative?"

- Lodash → Built-in methods (0KB)
- Moment.js → date-fns or Intl API (-70KB)
- Material UI → Shadcn UI (-110KB)
- Full Lucide → Individual icons (-100KB)

---

## PRINCIPLE 4: MOBILE-FIRST RESPONSIVE (Design for 375px, Enhance for Desktop)

**Problem:** Designs looked great on desktop but broke on mobile.

**Solution:** Always start with mobile design, progressively enhance for larger screens.

---

### ✅ CORRECT Application

```tsx
// ✅ GOOD: Mobile-first responsive classes
<section className="
  px-4 py-8           // Mobile: tight spacing
  md:px-8 md:py-12    // Tablet: more breathing room
  lg:px-16 lg:py-16   // Desktop: generous spacing
">
  <div className="
    grid
    grid-cols-1           // Mobile: stacked
    md:grid-cols-2        // Tablet: 2 columns
    lg:grid-cols-3        // Desktop: 3 columns
    gap-6 lg:gap-12
  ">
    {features.map(feature => <FeatureCard {...feature} />)}
  </div>
</section>

// Mobile: Single column, small spacing
// Tablet: 2 columns, medium spacing
// Desktop: 3 columns, large spacing
```

---

### ❌ WRONG Application

```tsx
// ❌ BAD: Desktop-first (mobile breaks)
<section className="px-16 py-16 lg:px-4 lg:py-8">
  <div className="grid grid-cols-3 gap-12 md:grid-cols-1 md:gap-6">
    {/* Features */}
  </div>
</section>

// Problems:
// - Mobile gets 64px padding (too much on small screens)
// - Grid starts at 3 columns (breaks on mobile)
// - Logic backwards (desktop → mobile)
```

---

### Mobile Testing

**Test on:**
- iPhone SE: 375px width (smallest modern phone)
- iPad: 768px width (tablet breakpoint)
- Desktop: 1440px width (common desktop)

---

### Test

**Before shipping, verify:**
> "Does this design work perfectly on a 375px screen?"

If no → Fix mobile first, then enhance.

---

## PRINCIPLE 5: SEMANTIC HTML, Not Div Soup

**Problem:** Components were built entirely with divs and spans, harming accessibility and SEO.

**Solution:** Use proper HTML5 semantic elements.

---

### ✅ CORRECT Application

```tsx
// ✅ GOOD: Semantic HTML structure
<article>
  <header>
    <h1>Article Title</h1>
    <p className="text-muted-foreground">Published on Nov 17, 2025</p>
  </header>

  <section aria-labelledby="features-heading">
    <h2 id="features-heading">Key Features</h2>
    <ul>
      <li>Feature 1</li>
      <li>Feature 2</li>
    </ul>
  </section>

  <footer>
    <nav aria-label="Article navigation">
      <a href="/previous">Previous</a>
      <a href="/next">Next</a>
    </nav>
  </footer>
</article>
```

**Benefits:**
- Screen readers understand document structure
- SEO: Search engines index semantic HTML better
- Accessibility: Landmarks for navigation (header, nav, main, aside, footer)

---

### ❌ WRONG Application

```tsx
// ❌ BAD: Div soup
<div className="article">
  <div className="header">
    <div className="title">Article Title</div>
    <div className="date">Published on Nov 17, 2025</div>
  </div>

  <div className="section">
    <div className="heading">Key Features</div>
    <div>
      <div>Feature 1</div>
      <div>Feature 2</div>
    </div>
  </div>

  <div className="footer">
    <div className="nav">
      <div className="link">Previous</div>
      <div className="link">Next</div>
    </div>
  </div>
</div>
```

**Problems:**
- Screen readers can't identify structure
- No semantic meaning
- SEO penalized
- Harder to maintain

---

### Semantic Elements to Use

| Element | Use For |
|---------|---------|
| `<header>` | Page/section header |
| `<nav>` | Navigation links |
| `<main>` | Main content (one per page) |
| `<article>` | Independent content (blog post, product) |
| `<section>` | Thematic grouping |
| `<aside>` | Sidebar, related content |
| `<footer>` | Page/section footer |
| `<button>` | Interactive buttons |
| `<a>` | Links (navigation) |

---

### Test

**Before shipping, ask:**
> "Could I replace this `<div>` with a semantic element?"

If yes → Use semantic element.

---

## Quality Enforcement Workflow

### When Generating Designs:

1. **READ this file** before starting
2. **Apply all 5 principles** during generation
3. **Self-validate** against each principle
4. **Report compliance** in metadata

---

### Example Metadata Output:

```json
{
  "5_principles_compliance": {
    "compose_not_copypaste": {
      "status": "PASS",
      "note": "Used Shadcn UI Button, Card primitives"
    },
    "accessibility_first": {
      "status": "PASS",
      "note": "Color contrast 4.8:1, focus indicators visible, semantic HTML"
    },
    "performance_budget": {
      "status": "PASS",
      "note": "Bundle: 87KB (< 90KB target), named imports only"
    },
    "mobile_first": {
      "status": "PASS",
      "note": "Tested 375px, responsive classes applied"
    },
    "semantic_html": {
      "status": "PASS",
      "note": "Used article, header, nav, section elements"
    }
  }
}
```

---

## Consequences of Violating Principles

**IF any principle is violated:**
1. Design DOES NOT ship
2. Agent must fix violations
3. Re-validate before delivery

**NO EXCEPTIONS** - These principles are mandatory for quality.

---

## Summary Checklist

Before shipping ANY design:

- ✅ **PRINCIPLE 1:** Used Shadcn UI primitives (not custom recreations)
- ✅ **PRINCIPLE 2:** Passed WCAG 2.2 AA (color contrast, keyboard, semantic HTML)
- ✅ **PRINCIPLE 3:** Bundle < 90KB (named imports, dynamic loading)
- ✅ **PRINCIPLE 4:** Works on 375px mobile (tested, responsive classes)
- ✅ **PRINCIPLE 5:** Semantic HTML used (article, nav, section, etc.)

**If all ✅ → Ship**

**If any ❌ → Fix before shipping**

---

**Last Updated:** November 17, 2025

**Version:** 1.0 (Mirrors Supah Copy Mazta's 5 Critical Lessons enforcement)
