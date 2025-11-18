# React Best Practices

**Purpose:** React composition patterns, performance optimization, and coding standards for Design Mode

---

## Composition Over Inheritance

**Principle:** Build complex components by composing simple ones, not extending classes.

```tsx
// ✅ GOOD: Composition
import { Button } from "@/components/ui/button"
import { Card } from "@/components/ui/card"

function FeatureCard({ icon: Icon, title, description }) {
  return (
    <Card>
      <Icon />
      <h3>{title}</h3>
      <p>{description}</p>
    </Card>
  )
}

// ❌ BAD: Inheritance (avoid class components)
class FeatureCard extends React.Component {
  render() { /* ... */ }
}
```

---

## Props Destructuring

**Principle:** Destructure props for clarity and type safety.

```tsx
// ✅ GOOD: Destructured props
interface HeroProps {
  heading: string
  subheading: string
  ctaText: string
}

export function Hero({ heading, subheading, ctaText }: HeroProps) {
  return <h1>{heading}</h1>
}

// ❌ BAD: Props object
export function Hero(props: HeroProps) {
  return <h1>{props.heading}</h1>
}
```

---

## TypeScript Interface Patterns

**Principle:** All props typed, no `any`, interfaces exported.

```tsx
// ✅ GOOD: Exported interface, strict types
export interface PricingCardProps {
  tier: string
  price: number
  features: string[]
  ctaText: string
  ctaHref: string
  popular?: boolean  // Optional with ?
}

export function PricingCard({
  tier,
  price,
  features,
  ctaText,
  ctaHref,
  popular = false  // Default value
}: PricingCardProps) {
  // ...
}

// ❌ BAD: Inline types, any
function PricingCard(props: any) {
  // ...
}
```

---

## Performance Optimization

### 1. Import Optimization (Tree-Shaking)

```tsx
// ✅ GOOD: Named imports (tree-shakeable)
import { Button } from '@/components/ui/button'
import { ArrowRight, Check } from 'lucide-react'

// ❌ BAD: Wildcard imports (kills tree-shaking)
import * as UI from '@/components/ui'
import * as Icons from 'lucide-react'
```

---

### 2. Dynamic Imports (Code Splitting)

```tsx
// ✅ GOOD: Dynamic import for below-fold content
import dynamic from 'next/dynamic'

const Pricing = dynamic(() => import('@/components/Pricing'))
const Testimonials = dynamic(() => import('@/components/Testimonials'))
const FAQ = dynamic(() => import('@/components/FAQ'))

export function LandingPage() {
  return (
    <>
      <Hero />  {/* Above fold: static */}
      <Pricing />  {/* Below fold: lazy loaded */}
      <Testimonials />
      <FAQ />
    </>
  )
}

// ❌ BAD: Import everything upfront
import Pricing from '@/components/Pricing'
import Testimonials from '@/components/Testimonials'
import FAQ from '@/components/FAQ'
```

---

### 3. Image Optimization

```tsx
// ✅ GOOD: next/image with optimization
import Image from 'next/image'

<Image
  src="/hero.png"
  alt="Product screenshot"
  width={1200}
  height={800}
  priority  // Above fold = preload
  placeholder="blur"
  blurDataURL="data:image/..."
/>

// Below fold: lazy load
<Image
  src="/feature.png"
  alt="Feature illustration"
  width={600}
  height={400}
  loading="lazy"
/>

// ❌ BAD: Regular img tag (no optimization)
<img src="/hero.png" alt="Product" />
```

---

### 4. Avoid Unnecessary Re-renders

```tsx
// ✅ GOOD: React.memo for expensive components
import { memo } from 'react'

export const PricingCard = memo(function PricingCard({
  tier,
  price,
  features
}: PricingCardProps) {
  // Expensive rendering logic
  return <Card>{/* ... */}</Card>
})

// ✅ GOOD: useCallback for event handlers passed to children
import { useCallback } from 'react'

function Parent() {
  const handleClick = useCallback(() => {
    console.log('clicked')
  }, [])

  return <Child onClick={handleClick} />
}

// ❌ BAD: New function on every render
function Parent() {
  return <Child onClick={() => console.log('clicked')} />
}
```

---

## Keys on Mapped Elements

**Principle:** Always provide stable keys when mapping arrays.

```tsx
// ✅ GOOD: Unique, stable key
{features.map((feature) => (
  <FeatureCard key={feature.id} {...feature} />
))}

// ✅ ACCEPTABLE: Index as key (if list never reorders)
{features.map((feature, index) => (
  <FeatureCard key={index} {...feature} />
))}

// ❌ BAD: No key
{features.map((feature) => (
  <FeatureCard {...feature} />
))}
```

---

## Server Components (Next.js 15 / React 19)

**Principle:** Use Server Components by default, Client Components only when needed.

```tsx
// ✅ GOOD: Server Component (default in Next.js 15)
// No "use client" directive = Server Component
export function Hero({ heading, subheading }) {
  return (
    <section>
      <h1>{heading}</h1>
      <p>{subheading}</p>
    </section>
  )
}

// ✅ GOOD: Client Component (when needed for interactivity)
"use client"

import { useState } from 'react'

export function MobileMenu() {
  const [isOpen, setIsOpen] = useState(false)

  return (
    <nav>
      <button onClick={() => setIsOpen(!isOpen)}>Menu</button>
      {/* ... */}
    </nav>
  )
}
```

**When to use Client Components:**
- `useState`, `useEffect`, event handlers
- Browser-only APIs (window, localStorage)
- Third-party libraries requiring client-side JS (Framer Motion)

---

## Accessibility Patterns

### Focus Management

```tsx
// ✅ GOOD: Focus indicators visible
<Button className="
  focus:outline-none
  focus-visible:ring-2
  focus-visible:ring-primary
  focus-visible:ring-offset-2
">
  Click me
</Button>

// ❌ BAD: Hidden focus (never do this)
<Button className="focus:outline-none">
  Click me
</Button>
```

---

### ARIA Attributes

```tsx
// ✅ GOOD: Proper ARIA for custom components
<button
  aria-expanded={isOpen}
  aria-controls="menu"
  aria-label="Toggle navigation menu"
>
  <MenuIcon aria-hidden="true" />
</button>

// ✅ GOOD: Form error handling
<Input
  id="email"
  aria-invalid={hasError ? "true" : "false"}
  aria-describedby={hasError ? "email-error" : undefined}
/>
{hasError && (
  <p id="email-error" role="alert">
    Invalid email address
  </p>
)}
```

---

## Styling with Tailwind CSS

### Mobile-First Responsive

```tsx
// ✅ GOOD: Mobile-first (base = mobile, then breakpoints)
<section className="
  px-4 py-8           // Mobile
  md:px-8 md:py-12    // Tablet (768px+)
  lg:px-16 lg:py-16   // Desktop (1024px+)
">
  Content
</section>

// ❌ BAD: Desktop-first
<section className="
  lg:px-16 lg:py-16
  md:px-8 md:py-12
  px-4 py-8
">
  Content
</section>
```

---

### Dark Mode

```tsx
// ✅ GOOD: Dark mode classes
<div className="
  bg-white dark:bg-slate-900
  text-slate-900 dark:text-slate-50
  border border-slate-200 dark:border-slate-800
">
  Content
</div>

// ✅ GOOD: Use semantic color names (adapts to dark mode)
<p className="text-muted-foreground">
  Subheading
</p>
```

---

### No Arbitrary Values (Use Design Tokens)

```tsx
// ✅ GOOD: Design tokens from Tailwind config
<div className="bg-primary text-primary-foreground">
  Content
</div>

// ❌ BAD: Arbitrary hex values (bypasses design system)
<div className="bg-[#2563eb] text-[#ffffff]">
  Content
</div>
```

---

## File Organization

```
components/
├── ui/                    # Shadcn UI primitives (copy-paste)
│   ├── button.tsx
│   ├── card.tsx
│   ├── input.tsx
│   └── ...
│
├── Hero.tsx               # Page sections (composed from ui/)
├── Features.tsx
├── Pricing.tsx
└── ...

lib/
├── utils.ts               # Utility functions
└── ...

app/
├── layout.tsx             # Root layout
├── page.tsx               # Homepage
└── ...
```

---

## Code Quality Checklist

Before shipping a component, verify:

- ✅ TypeScript types complete (no `any`)
- ✅ Props interface exported
- ✅ Functional component with hooks (not class)
- ✅ Props destructured
- ✅ Keys on mapped elements
- ✅ Named imports (tree-shakeable)
- ✅ Images use next/image
- ✅ Below-fold components lazy loaded
- ✅ Responsive classes applied (sm:, md:, lg:)
- ✅ Dark mode classes added
- ✅ Focus indicators visible
- ✅ ARIA attributes on interactive elements
- ✅ No console.log or debug code

---

**Last Updated:** November 17, 2025
