# Typography Systems

**Purpose:** Font selection, type scale generation, and typography best practices

---

## Font Selection

### Option 1: Inter Variable (Default - Recommended)

**Why:**
- Excellent readability at all sizes
- Modern, neutral, professional
- Variable font = single file, all weights (~25KB)
- Used by: GitHub, Vercel, Linear, Stripe

**Use For:** 90% of cases (B2B SaaS, developer tools, professional services)

**Installation:**

```tsx
// Using next/font (recommended)
import { Inter } from 'next/font/google'

const inter = Inter({
  subsets: ['latin'],
  variable: '--font-sans',
  display: 'swap',
  weight: ['400', '500', '600', '700'],  // Or use 'variable' for all weights
})

// In layout
<html className={inter.variable}>
  <body className="font-sans">{children}</body>
</html>
```

**Tailwind config:**
```javascript
{
  fontFamily: {
    sans: ['var(--font-sans)', ...fontFamily.sans],
  }
}
```

---

### Option 2: Poppins / Outfit (Geometric, Friendly)

**Why:**
- Friendly, modern, approachable
- Geometric letterforms
- Great for B2C, creative tools

**Use For:** B2C products, creative tools, playful brands

**Installation:**
```tsx
import { Poppins } from 'next/font/google'

const poppins = Poppins({
  subsets: ['latin'],
  variable: '--font-sans',
  display: 'swap',
  weight: ['400', '500', '600', '700', '800'],
})
```

---

### Option 3: Playfair Display + Lora (Classic, Elegant)

**Why:**
- Sophisticated, editorial feel
- Serif fonts convey authority
- Great for content-heavy sites

**Use For:** Premium products, publishing, content platforms

**Installation:**
```tsx
import { Playfair_Display, Lora } from 'next/font/google'

const playfair = Playfair_Display({
  subsets: ['latin'],
  variable: '--font-heading',
  display: 'swap',
})

const lora = Lora({
  subsets: ['latin'],
  variable: '--font-body',
  display: 'swap',
})
```

**Tailwind config:**
```javascript
{
  fontFamily: {
    heading: ['var(--font-heading)', ...fontFamily.serif],
    sans: ['var(--font-body)', ...fontFamily.serif],
  }
}
```

---

## Type Scale Generation

### 1.25 - Major Third (Moderate Contrast)

**Best For:** Minimal & Clean, Playful Minimalism

```javascript
{
  fontSize: {
    'xs': ['0.64rem', { lineHeight: '1rem' }],      // 10.24px
    'sm': ['0.8rem', { lineHeight: '1.25rem' }],    // 12.8px
    'base': ['1rem', { lineHeight: '1.5rem' }],     // 16px
    'lg': ['1.25rem', { lineHeight: '1.75rem' }],   // 20px
    'xl': ['1.563rem', { lineHeight: '2rem' }],     // 25px
    '2xl': ['1.953rem', { lineHeight: '2.25rem' }], // 31.25px
    '3xl': ['2.441rem', { lineHeight: '2.5rem' }],  // 39.06px
    '4xl': ['3.052rem', { lineHeight: '3rem' }],    // 48.83px
    '5xl': ['3.815rem', { lineHeight: '1' }],       // 61.04px
    '6xl': ['4.768rem', { lineHeight: '1' }],       // 76.29px
  }
}
```

**Visual hierarchy:** Moderate, professional, not too dramatic

---

### 1.333 - Perfect Fourth (More Contrast)

**Best For:** Bold & Colorful

```javascript
{
  fontSize: {
    'xs': ['0.563rem', { lineHeight: '1rem' }],     // 9px
    'sm': ['0.75rem', { lineHeight: '1.25rem' }],   // 12px
    'base': ['1rem', { lineHeight: '1.5rem' }],     // 16px
    'lg': ['1.333rem', { lineHeight: '1.75rem' }],  // 21.33px
    'xl': ['1.777rem', { lineHeight: '2rem' }],     // 28.43px
    '2xl': ['2.369rem', { lineHeight: '2.5rem' }],  // 37.9px
    '3xl': ['3.157rem', { lineHeight: '3rem' }],    // 50.52px
    '4xl': ['4.209rem', { lineHeight: '3.5rem' }],  // 67.34px
    '5xl': ['5.61rem', { lineHeight: '1' }],        // 89.76px
    '6xl': ['7.478rem', { lineHeight: '1' }],       // 119.65px
  }
}
```

**Visual hierarchy:** Strong, dramatic, energetic

---

### 1.2 - Minor Third (Subtle Contrast)

**Best For:** Dense content, dashboards, data-heavy apps

```javascript
{
  fontSize: {
    'xs': ['0.694rem', { lineHeight: '1rem' }],     // 11.1px
    'sm': ['0.833rem', { lineHeight: '1.25rem' }],  // 13.33px
    'base': ['1rem', { lineHeight: '1.5rem' }],     // 16px
    'lg': ['1.2rem', { lineHeight: '1.75rem' }],    // 19.2px
    'xl': ['1.44rem', { lineHeight: '2rem' }],      // 23.04px
    '2xl': ['1.728rem', { lineHeight: '2.25rem' }], // 27.65px
    '3xl': ['2.074rem', { lineHeight: '2.5rem' }],  // 33.18px
    '4xl': ['2.488rem', { lineHeight: '3rem' }],    // 39.81px
    '5xl': ['2.986rem', { lineHeight: '1' }],       // 47.78px
    '6xl': ['3.583rem', { lineHeight: '1' }],       // 57.33px
  }
}
```

**Visual hierarchy:** Subtle, compact, information-dense

---

## Font Weights

### Standard Weight Scale

```javascript
{
  fontWeight: {
    light: 300,       // Rarely used (accessibility concerns)
    normal: 400,      // Body text
    medium: 500,      // Emphasized body text
    semibold: 600,    // Subheadings, important text
    bold: 700,        // Headings, CTAs
    extrabold: 800,   // Display text (Bold & Colorful aesthetic)
    black: 900,       // Special cases only
  }
}
```

---

### Weight Usage Guidelines

**Body Text:**
- Normal weight (400) for readability
- Medium (500) for emphasis within paragraphs

**Headings:**
- Minimal & Clean: Bold (700)
- Bold & Colorful: Extrabold (800)
- Playful Minimalism: Semibold (600)
- Glassmorphism: Bold (700)

**Buttons & CTAs:**
- Medium (500) or Semibold (600)
- Never light (poor readability at small sizes)

---

## Line Height

### General Guidelines

```javascript
{
  lineHeight: {
    none: '1',          // Display text, hero headlines
    tight: '1.25',      // Large headings
    snug: '1.375',      // Section headings
    normal: '1.5',      // Body text (default)
    relaxed: '1.625',   // Long-form content
    loose: '2',         // Poetry, special cases
  }
}
```

---

### Usage by Element

```tsx
// Hero headline (tight)
<h1 className="text-6xl font-bold leading-none">
  Short Headline
</h1>

// Section heading (snug)
<h2 className="text-3xl font-bold leading-snug">
  Features That Make You Productive
</h2>

// Body text (normal)
<p className="text-base leading-normal">
  This is standard body text with comfortable line height.
</p>

// Long-form content (relaxed)
<article className="prose leading-relaxed">
  Long article content...
</article>
```

---

## Letter Spacing (Tracking)

### Guidelines

```javascript
{
  letterSpacing: {
    tighter: '-0.05em',   // Display text, large headings
    tight: '-0.025em',    // Headings (Minimal & Clean)
    normal: '0',          // Body text, most cases
    wide: '0.025em',      // Uppercase text, small caps
    wider: '0.05em',      // All-caps headings
    widest: '0.1em',      // Labels, tags
  }
}
```

---

### Usage

```tsx
// Tight tracking for large headings (more elegant)
<h1 className="text-6xl font-bold tracking-tight">
  Headline
</h1>

// Wide tracking for uppercase labels
<span className="text-xs uppercase tracking-wider font-semibold">
  NEW FEATURE
</span>

// Normal tracking for body
<p className="tracking-normal">
  Regular text
</p>
```

---

## Responsive Typography

### Mobile-First Scale

```tsx
// Hero headline: responsive sizing
<h1 className="
  text-4xl        // Mobile: 36px
  md:text-5xl     // Tablet: 48px
  lg:text-6xl     // Desktop: 60px
  font-bold
  tracking-tight
">
  Responsive Headline
</h1>

// Body text: slightly larger on desktop
<p className="
  text-base       // Mobile: 16px
  lg:text-lg      // Desktop: 18px
  leading-relaxed
">
  Body content
</p>

// Section heading
<h2 className="
  text-2xl        // Mobile: 24px
  md:text-3xl     // Tablet: 30px
  lg:text-4xl     // Desktop: 36px
  font-semibold
">
  Section Title
</h2>
```

---

## Accessibility Considerations

### Minimum Font Sizes

```
Body text:     ≥ 16px (1rem) - MANDATORY
Small text:    ≥ 14px (0.875rem) - Use sparingly
Legal text:    ≥ 12px (0.75rem) - Absolute minimum
```

**Never go below 12px** - WCAG guidelines for readability

---

### Line Length (Measure)

**Optimal:** 50-75 characters per line

```tsx
// ✅ GOOD: Max-width for readability
<p className="max-w-prose text-base leading-relaxed">
  Long-form content with optimal line length...
</p>

// Tailwind's `max-w-prose` = ~65ch (characters)

// ❌ BAD: Full-width text (hard to read)
<p className="w-full text-base">
  Very long lines are hard to read and cause eye strain...
</p>
```

---

## Font Loading Strategy

### Use `font-display: swap`

**Prevents FOIT (Flash of Invisible Text)**

```tsx
import { Inter } from 'next/font/google'

const inter = Inter({
  subsets: ['latin'],
  variable: '--font-sans',
  display: 'swap',  // ← Shows fallback font immediately
})
```

---

### Subset Fonts (Reduce File Size)

```tsx
const inter = Inter({
  subsets: ['latin'],  // Don't load Cyrillic/Greek if not needed
  weight: ['400', '600', '700'],  // Only weights you use
})
```

**Performance:**
- Full Inter Variable: ~150KB
- Subsetted (Latin, 3 weights): ~25KB

---

### Preload Critical Fonts

```tsx
// In layout or head
<link
  rel="preload"
  href="/fonts/inter-var.woff2"
  as="font"
  type="font/woff2"
  crossOrigin="anonymous"
/>
```

---

## Typography Utilities (Tailwind CSS)

### Prose (Long-Form Content)

```tsx
import '@tailwindcss/typography'

// Automatic article styling
<article className="prose lg:prose-xl dark:prose-invert">
  <h1>Article Title</h1>
  <p>Article content with automatic styling...</p>
  <ul>
    <li>List items styled</li>
  </ul>
</article>
```

**Customization:**
```javascript
// tailwind.config.ts
{
  plugins: [require('@tailwindcss/typography')],
  theme: {
    extend: {
      typography: {
        DEFAULT: {
          css: {
            color: '#334155',  // slate-700
            a: {
              color: '#2563eb',  // Primary color
              '&:hover': {
                color: '#1d4ed8',
              },
            },
          },
        },
      },
    },
  },
}
```

---

## Typography Checklist

Before finalizing typography system:

- ✅ Font family chosen (matches aesthetic)
- ✅ Variable font used (if available) for performance
- ✅ Type scale generated (consistent ratio)
- ✅ Line heights defined for all sizes
- ✅ Font weights limited to 3-4 values (not full range)
- ✅ Letter spacing applied to headings
- ✅ Minimum 16px body text (accessibility)
- ✅ Responsive scaling defined (mobile → desktop)
- ✅ Max-width set for long-form content (readability)
- ✅ `font-display: swap` enabled (performance)
- ✅ Fonts subsetted (only needed characters/weights)
- ✅ Critical fonts preloaded

---

**Last Updated:** November 17, 2025
