# Image Optimization

**Purpose:** Image optimization strategies for performance and Core Web Vitals

---

## Image Formats

### Format Comparison

| Format | Use Case | Size | Browser Support |
|--------|----------|------|-----------------|
| **AVIF** | Photos, modern browsers | Smallest (~30% smaller than WebP) | Chrome 85+, Firefox 86+ |
| **WebP** | Photos, graphics | Small (~25% smaller than JPEG) | All modern browsers |
| **JPEG** | Photos | Medium | Universal |
| **PNG** | Graphics with transparency | Large | Universal |
| **SVG** | Icons, logos, illustrations | Tiny (vector) | Universal |

---

### Recommended Strategy

```tsx
// Next.js automatically serves best format
import Image from 'next/image'

<Image
  src="/hero.jpg"
  alt="Hero image"
  width={1200}
  height={800}
/>

// Next.js generates:
// - AVIF for modern browsers
// - WebP for older browsers
// - JPEG as fallback
```

---

## next/image Component

### Basic Usage

```tsx
import Image from 'next/image'

// ✅ GOOD: Above-fold image (priority)
<Image
  src="/hero.jpg"
  alt="Product dashboard"
  width={1200}
  height={800}
  priority  // Preload (LCP optimization)
  placeholder="blur"  // Shows blur while loading
  blurDataURL="data:image/jpeg;base64,..."  // Tiny blur image
/>

// ✅ GOOD: Below-fold image (lazy load)
<Image
  src="/feature.jpg"
  alt="Feature screenshot"
  width={600}
  height={400}
  loading="lazy"  // Load when near viewport
/>
```

---

### Sizes Attribute (Responsive)

```tsx
// ✅ GOOD: Responsive srcset
<Image
  src="/hero.jpg"
  alt="Hero"
  width={1200}
  height={800}
  sizes="(max-width: 768px) 100vw, (max-width: 1200px) 50vw, 33vw"
  // Mobile: full width
  // Tablet: 50% width
  // Desktop: 33% width
/>

// Next.js generates multiple sizes:
// hero-640.jpg, hero-750.jpg, hero-828.jpg, hero-1080.jpg, hero-1200.jpg
```

---

### Fill Mode (Unknown Dimensions)

```tsx
// ✅ GOOD: Fill container (for dynamic images)
<div className="relative h-96 w-full">
  <Image
    src="/background.jpg"
    alt="Background"
    fill
    className="object-cover"  // cover, contain, fill
    sizes="100vw"
  />
</div>
```

---

## Image Compression

### Quality Settings

```javascript
// next.config.js
module.exports = {
  images: {
    quality: 85,  // Default: 75 (Range: 1-100)
    // 85 = Good balance (slight quality loss, 30% smaller)
    // 90 = High quality (minimal loss, 20% smaller)
    // 75 = Lower quality (noticeable loss, 40% smaller)
  },
}
```

**Recommendations:**
- **Photos:** 80-85 quality
- **Graphics/screenshots:** 85-90 quality
- **Backgrounds:** 70-80 quality

---

### Manual Compression Tools

**Before uploading:**
- **TinyPNG/TinyJPG:** https://tinypng.com/ (70-80% reduction)
- **Squoosh:** https://squoosh.app/ (Google tool, custom settings)
- **ImageOptim (Mac):** https://imageoptim.com/mac

---

## Responsive Images

### Breakpoints

```tsx
// ✅ GOOD: Different images for different screens
import Image from 'next/image'

function Hero() {
  return (
    <picture>
      {/* Desktop */}
      <source
        media="(min-width: 1024px)"
        srcSet="/hero-desktop.jpg"
      />
      {/* Tablet */}
      <source
        media="(min-width: 768px)"
        srcSet="/hero-tablet.jpg"
      />
      {/* Mobile */}
      <Image
        src="/hero-mobile.jpg"
        alt="Hero"
        width={640}
        height={800}
        priority
      />
    </picture>
  )
}
```

---

### Art Direction

```tsx
// Different crops for different screens
<picture>
  {/* Desktop: landscape crop */}
  <source
    media="(min-width: 1024px)"
    srcSet="/hero-landscape.jpg"
  />
  {/* Mobile: portrait crop */}
  <Image
    src="/hero-portrait.jpg"
    alt="Hero"
    width={640}
    height={800}
  />
</picture>
```

---

## LCP (Largest Contentful Paint) Optimization

### Identify LCP Element

```bash
# Use Chrome DevTools
# 1. Open DevTools
# 2. Performance tab
# 3. Record page load
# 4. Find LCP marker (usually hero image)
```

---

### Optimize LCP Image

```tsx
// ✅ GOOD: Preload LCP image
<Image
  src="/hero.jpg"
  alt="Hero"
  width={1200}
  height={800}
  priority  // <link rel="preload"> added to <head>
  fetchPriority="high"
/>

// Next.js generates:
// <link rel="preload" as="image" href="/hero.jpg" fetchpriority="high">
```

**Target:** LCP < 2.0s (excellent), < 2.5s (good)

---

## Image Sizing

### Recommended Sizes

```
Hero Images:
- Desktop: 1920x1080 or 2400x1350 (2x for Retina)
- Tablet: 1200x675
- Mobile: 750x1000 (portrait) or 750x422 (landscape)

Feature Images:
- Desktop: 800x600 or 1200x900
- Mobile: 600x450

Icons/Logos:
- SVG (vector, infinite resolution)
- Or PNG: 200x200 (1x), 400x400 (2x Retina)

Thumbnails:
- 300x300 or 400x400
```

---

### Aspect Ratios

```tsx
// Maintain aspect ratio to prevent CLS (Cumulative Layout Shift)

// ✅ GOOD: Explicit width/height
<Image
  src="/feature.jpg"
  alt="Feature"
  width={800}
  height={600}  // 4:3 aspect ratio
/>

// Next.js reserves space, prevents CLS

// ❌ BAD: No dimensions (causes layout shift)
<img src="/feature.jpg" alt="Feature" />
```

---

## SVG Optimization

### Inline SVG (Small icons)

```tsx
// ✅ GOOD: Inline SVG for icons
export function CheckIcon() {
  return (
    <svg
      xmlns="http://www.w3.org/2000/svg"
      width="24"
      height="24"
      viewBox="0 0 24 24"
      fill="none"
      stroke="currentColor"
      className="text-green-600"
    >
      <path d="M5 13l4 4L19 7" />
    </svg>
  )
}

// Benefits:
// - No HTTP request
// - Can style with CSS (fill, stroke, color)
// - Tiny size (~500 bytes)
```

---

### Use Icon Libraries

```tsx
// ✅ GOOD: Lucide React (tree-shakeable)
import { Check, ArrowRight } from 'lucide-react'

<Check className="h-5 w-5 text-green-600" />
<ArrowRight className="h-4 w-4" />

// Only includes icons you import (~0.5KB each)
```

---

### SVGO (SVG Optimizer)

```bash
# Optimize SVG files
npm install -g svgo

svgo input.svg -o output.svg

# Removes:
# - Unnecessary metadata
# - Comments
# - Hidden elements
# - Default values

# Result: 30-70% smaller
```

---

## Background Images

### Use CSS (Not img tag)

```tsx
// ✅ GOOD: CSS background (lazy loads by default)
<div
  className="h-screen bg-cover bg-center"
  style={{ backgroundImage: "url('/background.jpg')" }}
>
  Content
</div>

// Or with Tailwind arbitrary value
<div className="h-screen bg-[url('/background.jpg')] bg-cover bg-center">
  Content
</div>
```

---

### Optimize Background Images

```tsx
// ✅ GOOD: Compressed, blurred background
// 1. Compress heavily (quality: 60-70)
// 2. Apply slight blur in Photoshop/Figma
// 3. Blur hides compression artifacts

<div
  className="h-screen bg-cover bg-center"
  style={{
    backgroundImage: "url('/background-compressed.jpg')",
    filter: "blur(0.5px)",  // Optional: Additional blur
  }}
>
  Content
</div>
```

---

## Loading States

### Blur Placeholder

```tsx
// ✅ GOOD: Generate blur placeholder
import Image from 'next/image'
import { getPlaiceholder } from 'plaiceholder'

// Generate at build time
export async function getStaticProps() {
  const { base64 } = await getPlaiceholder('/hero.jpg')

  return {
    props: { blurDataURL: base64 },
  }
}

// Use in component
<Image
  src="/hero.jpg"
  alt="Hero"
  width={1200}
  height={800}
  placeholder="blur"
  blurDataURL={blurDataURL}
/>
```

---

### Shimmer Effect

```tsx
// ✅ GOOD: Animated shimmer while loading
const shimmer = `
  relative overflow-hidden
  before:absolute
  before:inset-0
  before:-translate-x-full
  before:animate-shimmer
  before:bg-gradient-to-r
  before:from-transparent
  before:via-white/10
  before:to-transparent
`

<div className={shimmer}>
  <Image src="/image.jpg" alt="..." />
</div>
```

---

## CLS (Cumulative Layout Shift) Prevention

### Always Provide Dimensions

```tsx
// ✅ GOOD: Prevents layout shift
<Image
  src="/feature.jpg"
  alt="Feature"
  width={800}
  height={600}
/>

// Space reserved: 800x600
// No shift when image loads

// ❌ BAD: Causes layout shift
<img src="/feature.jpg" alt="Feature" />

// No space reserved
// Content shifts when image loads (poor CLS)
```

**Target:** CLS < 0.05 (excellent), < 0.1 (good)

---

## Image CDN (Remote Images)

### Configure Domains

```javascript
// next.config.js
module.exports = {
  images: {
    remotePatterns: [
      {
        protocol: 'https',
        hostname: 'images.unsplash.com',
      },
      {
        protocol: 'https',
        hostname: 'cdn.example.com',
      },
    ],
  },
}
```

---

### Use Image CDN Features

```tsx
// Cloudinary example
<Image
  src="https://res.cloudinary.com/demo/image/upload/w_800,q_auto,f_auto/sample.jpg"
  alt="Sample"
  width={800}
  height={600}
/>

// w_800: Resize to 800px width
// q_auto: Automatic quality
// f_auto: Automatic format (WebP/AVIF)
```

---

## Image Optimization Checklist

Before shipping:

- ✅ All images use next/image (not <img>)
- ✅ Above-fold images have priority={true}
- ✅ Below-fold images lazy load (loading="lazy")
- ✅ All images have width/height (prevent CLS)
- ✅ All images have descriptive alt text
- ✅ Hero image is LCP element with priority
- ✅ Images compressed (TinyPNG or Squoosh)
- ✅ Quality setting: 80-85 for photos
- ✅ Responsive sizes attribute used
- ✅ SVG used for icons/logos (not PNG)
- ✅ Background images compressed heavily
- ✅ Blur placeholders generated
- ✅ No layout shift when images load (CLS < 0.05)

---

## Quick Wins (Biggest Impact)

1. **Use next/image** for all images (-40% size, automatic optimization)
2. **Priority on LCP image** (-0.5s LCP improvement)
3. **Compress images before upload** (-50-70% size via TinyPNG)
4. **Use SVG for icons** (-10KB vs. PNG)
5. **Lazy load below-fold images** (-30-50KB initial load)

---

**Last Updated:** November 17, 2025
