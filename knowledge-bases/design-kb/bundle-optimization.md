# Bundle Optimization

**Purpose:** JavaScript bundle size optimization strategies and best practices

---

## Performance Budgets

### Target Bundle Sizes (gzipped)

```
MVP (Landing Page):
- Total JS: < 90KB
- Total CSS: < 15KB
- LCP: < 2.0s
- FID: < 50ms
- CLS: < 0.05

Phase 2 (With Animations):
- Total JS: < 130KB
- Total CSS: < 20KB
- LCP: < 2.5s
- FID: < 100ms
- CLS: < 0.1
```

---

## Tree-Shaking Optimization

### Named Imports (Tree-Shakeable)

```tsx
// ✅ GOOD: Named imports (only what you use)
import { Button } from '@/components/ui/button'
import { Card } from '@/components/ui/card'
import { ArrowRight, Check } from 'lucide-react'

// Bundler includes ONLY:
// - Button component
// - Card component
// - ArrowRight icon
// - Check icon

// ❌ BAD: Wildcard imports (entire library)
import * as UI from '@/components/ui'
import * as Icons from 'lucide-react'

// Bundler includes EVERYTHING (200+ icons)
```

---

### Import Only What You Need

```tsx
// ✅ GOOD: Specific utilities
import { cn } from '@/lib/utils'

// ❌ BAD: Entire lodash library
import _ from 'lodash'
import { debounce } from 'lodash'  // Still imports entire lodash!

// ✅ GOOD: Specific lodash function
import debounce from 'lodash/debounce'

// ✅ BETTER: Use built-in or write custom
const debounce = (fn, delay) => {
  let timeout
  return (...args) => {
    clearTimeout(timeout)
    timeout = setTimeout(() => fn(...args), delay)
  }
}
```

---

## Code Splitting

### Dynamic Imports (Next.js)

```tsx
import dynamic from 'next/dynamic'

// ✅ GOOD: Lazy load below-fold components
const Pricing = dynamic(() => import('@/components/Pricing'))
const Testimonials = dynamic(() => import('@/components/Testimonials'))
const FAQ = dynamic(() => import('@/components/FAQ'))

export default function LandingPage() {
  return (
    <>
      {/* Above fold: static (included in initial bundle) */}
      <Hero />
      <Features />

      {/* Below fold: lazy loaded (separate chunks) */}
      <Pricing />
      <Testimonials />
      <FAQ />
    </>
  )
}
```

---

### Loading States

```tsx
// ✅ GOOD: With loading fallback
const Pricing = dynamic(() => import('@/components/Pricing'), {
  loading: () => (
    <div className="flex justify-center p-16">
      <Loader className="animate-spin" />
    </div>
  ),
})

// ✅ GOOD: Disable SSR for client-only components
const Chart = dynamic(() => import('@/components/Chart'), {
  ssr: false,  // Don't render on server (client-only)
})
```

---

### Route-Based Code Splitting

```tsx
// Next.js automatically splits routes
// Each page.tsx becomes a separate chunk

// app/page.tsx → home chunk
// app/about/page.tsx → about chunk
// app/pricing/page.tsx → pricing chunk

// No configuration needed!
```

---

## Library Size Awareness

### Common Library Sizes (minified + gzipped)

| Library | Size | Notes |
|---------|------|-------|
| React | ~45KB | Core library |
| React DOM | ~130KB | Rendering |
| Next.js Runtime | ~85KB | Framework overhead |
| Framer Motion | ~28KB | Animations |
| Shadcn UI (per component) | ~2-5KB | Copy-paste components |
| Radix UI Button | ~3KB | Primitive component |
| Lucide React (per icon) | ~0.5KB | Individual icons |
| Tailwind CSS | ~5-15KB | Generated CSS |
| Zod | ~12KB | Schema validation |

---

### Choose Lightweight Alternatives

```tsx
// Heavy: Moment.js (~70KB)
import moment from 'moment'
const date = moment().format('YYYY-MM-DD')

// ✅ Lightweight: date-fns (~2KB per function)
import { format } from 'date-fns'
const date = format(new Date(), 'yyyy-MM-dd')

// ✅ Best: Native Intl API (0KB)
const date = new Intl.DateTimeFormat('en-CA').format(new Date())
```

---

## Shadcn UI vs. Component Libraries

### Why Shadcn UI is Lean

```tsx
// ❌ Heavy: Install entire library
npm install @mui/material  // ~120KB

import { Button, Card, TextField } from '@mui/material'
// Bundles entire library even if you use 3 components

// ✅ Lean: Copy-paste only what you need
npx shadcn-ui add button card input

// Copies source code directly into your project
// components/ui/button.tsx  (~2KB)
// components/ui/card.tsx    (~3KB)
// components/ui/input.tsx   (~2KB)

// Total: ~7KB vs. 120KB
```

---

## Analyzing Bundle Size

### Next.js Bundle Analyzer

```bash
# Install
npm install @next/bundle-analyzer

# Configure in next.config.js
const withBundleAnalyzer = require('@next/bundle-analyzer')({
  enabled: process.env.ANALYZE === 'true',
})

module.exports = withBundleAnalyzer({
  // Next.js config
})

# Run analysis
ANALYZE=true npm run build
```

---

### Reading the Output

```
Route Size            Packages
────────────────────────────────────────
/ (Server)   45 KB   react, react-dom
/ (Client)   85 KB   next/client, framer-motion

/_app        12 KB   (shared across all pages)

/pricing     92 KB   includes Pricing component
/about       78 KB   includes About component

Shared by all:       85 KB
First Load JS:       130 KB  ← Target: < 150 KB
```

**Analysis:**
- First Load JS = Shared + Page
- Each subsequent page only loads page chunk
- Keep First Load JS < 150 KB (excellent performance)

---

## Font Optimization

### Use next/font (Next.js 13+)

```tsx
// ✅ GOOD: Variable font (all weights in one file)
import { Inter } from 'next/font/google'

const inter = Inter({
  subsets: ['latin'],
  variable: '--font-sans',
  display: 'swap',
})

// ~25KB for all weights (400, 500, 600, 700, etc.)

// ❌ BAD: Loading individual font files
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" />

// ~15KB per weight = 60KB total
```

---

### Subset Fonts

```tsx
const inter = Inter({
  subsets: ['latin'],  // Only Latin characters
  // Don't load: cyrillic, greek, vietnamese if not needed
  weight: ['400', '600', '700'],  // Only 3 weights
})

// Savings: 150KB → 25KB
```

---

## CSS Optimization

### Tailwind CSS Purging

```javascript
// tailwind.config.ts
export default {
  content: [
    './app/**/*.{ts,tsx}',
    './components/**/*.{ts,tsx}',
  ],
  // Purges unused classes automatically
}

// Unused classes are removed at build time
// Final CSS: ~5-15KB (vs. 3MB full Tailwind)
```

---

### Avoid CSS-in-JS Runtime

```tsx
// ❌ BAD: Runtime CSS-in-JS (adds ~20KB + runtime cost)
import styled from 'styled-components'
const Button = styled.button`
  background: blue;
  padding: 1rem;
`

// ✅ GOOD: Tailwind (compiled, 0 runtime cost)
<button className="bg-blue-600 px-4 py-2">
  Button
</button>
```

---

## Image Optimization (Bundle Impact)

### Use next/image (No Bundle Cost)

```tsx
// ✅ GOOD: next/image (0KB bundle cost, optimizes images)
import Image from 'next/image'

<Image
  src="/hero.jpg"
  alt="Hero image"
  width={1200}
  height={800}
  priority
/>

// Next.js generates WebP/AVIF automatically
// Lazy loads below-fold images
// No impact on JavaScript bundle
```

---

## Remove Dead Code

### TypeScript unused exports

```tsx
// ❌ BAD: Unused exports still bundled
export function usedFunction() { /* ... */ }
export function unusedFunction() { /* ... */ }  // Still included!

// ✅ GOOD: Only export what's used
export function usedFunction() { /* ... */ }
function unusedFunction() { /* ... */ }  // Tree-shaken out
```

---

### Remove console.log

```tsx
// ❌ BAD: console.log in production (small but adds up)
console.log('Debug info', data)

// ✅ GOOD: Use build-time removal
if (process.env.NODE_ENV === 'development') {
  console.log('Debug info', data)
}

// Or use babel-plugin-transform-remove-console
```

---

## Server Components (Next.js 15)

### Reduce Client-Side JS

```tsx
// ✅ GOOD: Server Component (0KB client JS)
// No "use client" directive
export default function Features() {
  return (
    <section>
      <h2>Features</h2>
      {/* Static content */}
    </section>
  )
}

// Only use Client Components when needed:
// - useState, useEffect
// - Event handlers
// - Browser APIs
```

---

### Client Component Only When Needed

```tsx
// ✅ GOOD: Minimal client JS
// Server Component
export default function LandingPage() {
  return (
    <>
      <Hero />  {/* Server Component */}
      <Features />  {/* Server Component */}
      <MobileMenu />  {/* Client Component (interactive) */}
    </>
  )
}

// Client Component (interactive)
"use client"
export function MobileMenu() {
  const [isOpen, setIsOpen] = useState(false)
  // ...
}
```

---

## Minification & Compression

### Automatic in Next.js

```bash
# Production build automatically:
# - Minifies JavaScript
# - Minifies CSS
# - Removes comments
# - Gzip/Brotli compression

npm run build
```

---

### Verify Compression

```bash
# Check gzipped size
du -sh .next/static/chunks/*.js | sort -h

# Example output:
# 12K  main-abc123.js
# 45K  framework-def456.js
# 85K  page-ghi789.js
```

---

## Bundle Optimization Checklist

Before shipping:

- ✅ Named imports used (no wildcard imports)
- ✅ Below-fold components lazy loaded (dynamic import)
- ✅ Shadcn UI used (not full component library)
- ✅ Individual Lucide icons imported (not wildcard)
- ✅ Variable fonts used (single file)
- ✅ Fonts subsetted (only Latin if applicable)
- ✅ Tailwind CSS purged (unused classes removed)
- ✅ No CSS-in-JS runtime (use Tailwind)
- ✅ Server Components used where possible
- ✅ Client Components only when needed
- ✅ No console.log in production
- ✅ Bundle analyzed (ANALYZE=true npm run build)
- ✅ First Load JS < 150KB (target met)
- ✅ Total bundle < 90KB (MVP) or < 130KB (Phase 2)

---

## Quick Wins (Biggest Impact)

1. **Use Shadcn UI** instead of Material UI (-110KB)
2. **Dynamic import below-fold components** (-30-50KB)
3. **Variable fonts** instead of multiple files (-35KB)
4. **Server Components** where possible (-20-40KB)
5. **Named imports only** (prevents accidental bloat)

---

**Last Updated:** November 17, 2025
