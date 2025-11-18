# DESIGN QUALITY CHECKLIST

**Purpose:** Final validation before delivering design to user

---

## Accessibility (WCAG 2.2 AA) - MANDATORY

**Color Contrast:**
- [ ] Text contrast ≥ 4.5:1 (normal text)
- [ ] Text contrast ≥ 3:1 (large text ≥ 24px)
- [ ] UI component contrast ≥ 3:1 (buttons, form inputs)
- [ ] Dark mode contrast validated

**Semantic HTML:**
- [ ] Proper heading hierarchy (h1 → h2 → h3, no skips)
- [ ] One h1 per page
- [ ] Semantic elements used (nav, main, article, section)
- [ ] No div soup

**ARIA & Labels:**
- [ ] All images have alt text (or alt="" if decorative)
- [ ] All form inputs have labels
- [ ] All interactive elements have accessible names
- [ ] ARIA attributes valid (no redundant roles)
- [ ] Skip link present

**Keyboard Navigation:**
- [ ] All interactive elements focusable (Tab key)
- [ ] Focus indicators visible (ring-2 ring-primary)
- [ ] Logical tab order
- [ ] No keyboard traps
- [ ] Modals closable with Escape

**Screen Reader:**
- [ ] Meaningful link text (no "click here")
- [ ] Form errors announced (role="alert")
- [ ] Decorative icons hidden (aria-hidden="true")

---

## Performance - MANDATORY

**Bundle Size:**
- [ ] Total bundle < 90KB gzipped (MVP)
- [ ] Individual components < 15KB each
- [ ] Tree-shaking verified (no wildcard imports)

**Images:**
- [ ] All images use next/image
- [ ] WebP/AVIF formats generated
- [ ] Above-fold images have priority
- [ ] Below-fold images lazy loaded
- [ ] Images compressed (80-85 quality)

**Fonts:**
- [ ] Variable fonts used (Inter Variable)
- [ ] font-display: swap set
- [ ] Subset to required characters (Latin)
- [ ] Total font weight < 30KB
- [ ] Preload critical fonts

**Code:**
- [ ] Code splitting implemented (above/below fold)
- [ ] Dynamic imports for heavy components
- [ ] No console.log or debug code
- [ ] Minification enabled

**Core Web Vitals:**
- [ ] Estimated LCP < 2.0s
- [ ] Estimated FID < 50ms
- [ ] Estimated CLS < 0.05

---

## Code Quality - MANDATORY

**TypeScript:**
- [ ] All props typed (no `any`)
- [ ] Interfaces exported
- [ ] Strict mode enabled
- [ ] No TypeScript errors

**React:**
- [ ] Functional components with hooks
- [ ] Props destructured
- [ ] Keys on mapped elements
- [ ] No unnecessary re-renders

**Styling:**
- [ ] Tailwind classes used (no inline styles)
- [ ] Responsive classes applied (sm:, md:, lg:)
- [ ] Dark mode classes added
- [ ] No arbitrary values (use design tokens)

**File Structure:**
- [ ] Components in /components
- [ ] UI primitives in /components/ui
- [ ] Utilities in /lib
- [ ] Types in interfaces or inline

---

## Responsive Design - MANDATORY

**Mobile (< 640px):**
- [ ] Single column layouts
- [ ] Touch targets ≥ 44x44px
- [ ] Text readable (≥ 16px base)
- [ ] No horizontal scroll

**Tablet (640px - 1024px):**
- [ ] 2-column grids where appropriate
- [ ] Adequate spacing
- [ ] Navigation accessible

**Desktop (≥ 1024px):**
- [ ] 3-column grids where appropriate
- [ ] Max-width containers (avoid super-wide text)
- [ ] Generous whitespace

**Testing:**
- [ ] Tested on mobile viewport
- [ ] Tested on tablet viewport
- [ ] Tested on desktop viewport

---

## Design System Consistency - MANDATORY

**Colors:**
- [ ] Only design tokens used (no hardcoded hex)
- [ ] Primary, secondary, accent consistent
- [ ] Neutral colors from defined scale

**Typography:**
- [ ] Font family from design tokens
- [ ] Font sizes from type scale
- [ ] Line heights appropriate
- [ ] Font weights from defined scale

**Spacing:**
- [ ] 4px base unit (Tailwind scale)
- [ ] Consistent gap values
- [ ] Logical padding/margin

**Components:**
- [ ] Shadcn UI primitives used
- [ ] Custom components composed from primitives
- [ ] No reinventing the wheel

---

## Content - RECOMMENDED

**Text:**
- [ ] No placeholder text (or clearly marked as placeholder)
- [ ] No lorem ipsum in final output
- [ ] Proper capitalization
- [ ] No typos

**Images:**
- [ ] Descriptive alt text
- [ ] Appropriate dimensions
- [ ] Aspect ratios maintained

**Links:**
- [ ] All links functional (or marked as placeholder)
- [ ] External links open in new tab (where appropriate)
- [ ] Descriptive link text

---

## Dark Mode - MANDATORY (if enabled)

**Colors:**
- [ ] Dark mode colors defined
- [ ] Contrast maintained (≥ 4.5:1)
- [ ] Not just inverted (adjusted for dark backgrounds)

**Components:**
- [ ] All components have dark: classes
- [ ] Focus indicators visible on dark backgrounds
- [ ] Shadows adjusted for dark mode

**Testing:**
- [ ] Tested in dark mode
- [ ] Toggle works
- [ ] Preference persisted

---

## Animations - RECOMMENDED (if enabled)

**Performance:**
- [ ] GPU-accelerated properties only (transform, opacity)
- [ ] No layout-triggering animations (width, height)
- [ ] Respects prefers-reduced-motion

**Timing:**
- [ ] Appropriate durations (100-800ms)
- [ ] Easing feels natural
- [ ] Not distracting

**Implementation:**
- [ ] Framer Motion used correctly
- [ ] Bundle impact validated (< 130KB total with animations)

---

## SEO - RECOMMENDED

**Metadata:**
- [ ] Page title defined
- [ ] Meta description defined
- [ ] Open Graph tags (og:title, og:description, og:image)
- [ ] Twitter Card tags

**Content:**
- [ ] Proper heading hierarchy (SEO + accessibility)
- [ ] Descriptive alt text on images
- [ ] Internal links where appropriate

---

## FINAL CHECKLIST

**Before Delivery:**
- [ ] All MANDATORY items checked
- [ ] No console errors
- [ ] No TypeScript errors
- [ ] Build succeeds
- [ ] README with setup instructions included

**Quality Score:**
- Accessibility: Pass/Fail (must pass)
- Performance: Pass/Fail (must pass)
- Code Quality: Pass/Fail (must pass)
- Responsive Design: Pass/Fail (must pass)

**If ANY mandatory item fails → Fix before delivery**

---

**REMEMBER:** This is the final gate. No design ships without passing all MANDATORY checks. RECOMMENDED items improve quality but aren't blockers.
