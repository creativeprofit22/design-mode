# Aesthetic Styles

**Purpose:** Complete specifications for the 4 aesthetic options in Design Mode

---

## 1. Minimal & Clean

**Characteristics:**
- Lots of whitespace
- Subtle shadows
- Neutral color palette (grays + one accent)
- Classic, readable typography (Inter, SF Pro)
- Simple, functional layouts
- No unnecessary ornamentation

**Best For:** B2B SaaS, developer tools, professional services

**Example Brands:** Vercel, Linear, Supabase, Stripe (docs), GitHub

---

### Color Palette

```javascript
{
  primary: {
    50: '#eff6ff',
    100: '#dbeafe',
    200: '#bfdbfe',
    300: '#93c5fd',
    400: '#60a5fa',
    500: '#3b82f6',
    600: '#2563eb',  // Main primary
    700: '#1d4ed8',
    800: '#1e40af',
    900: '#1e3a8a',
    950: '#172554',
  },
  neutral: 'slate',  // Use Tailwind's slate scale
  accent: '#6366f1',  // Indigo for subtle pops
}
```

**Why Blue:**
- Conveys trust and professionalism
- 60% of B2B SaaS use blue
- Associated with data/intelligence/technology

---

### Typography

```javascript
{
  fontFamily: {
    sans: ['Inter Variable', ...fontFamily.sans],
    heading: ['Inter Variable', ...fontFamily.sans],
  },
  fontSize: {
    // Type scale: 1.25 (Major Third - moderate contrast)
    'xs': '0.75rem',    // 12px
    'sm': '0.875rem',   // 14px
    'base': '1rem',     // 16px
    'lg': '1.125rem',   // 18px
    'xl': '1.25rem',    // 20px
    '2xl': '1.5rem',    // 24px
    '3xl': '1.875rem',  // 30px
    '4xl': '2.25rem',   // 36px
    '5xl': '3rem',      // 48px
    '6xl': '3.75rem',   // 60px
  },
  fontWeight: {
    body: 400,
    medium: 500,
    semibold: 600,
    bold: 700,
  },
  lineHeight: {
    body: 1.5,
    heading: 1.2,
  },
  letterSpacing: {
    tight: '-0.025em',
    normal: '0',
  }
}
```

**Font Loading:**
```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
```

Or use next/font (recommended):
```tsx
import { Inter } from 'next/font/google'

const inter = Inter({
  subsets: ['latin'],
  variable: '--font-sans',
  display: 'swap',
})
```

---

### Spacing & Layout

```javascript
{
  spacing: {
    // Tailwind default (4px base)
    // Generous whitespace
    section: 'py-16 md:py-24 lg:py-32',
    container: 'px-4 md:px-8 lg:px-16',
  },
  borderRadius: {
    sm: '0.25rem',  // 4px
    DEFAULT: '0.375rem',  // 6px
    lg: '0.5rem',  // 8px
  },
  boxShadow: {
    // Subtle elevation only
    sm: '0 1px 2px 0 rgb(0 0 0 / 0.05)',
    DEFAULT: '0 1px 3px 0 rgb(0 0 0 / 0.1)',
    md: '0 4px 6px -1px rgb(0 0 0 / 0.1)',
  }
}
```

---

### Example Component Styles

```tsx
// Hero section
<section className="bg-white dark:bg-slate-950">
  <div className="container mx-auto px-4 py-24 lg:py-32">
    <h1 className="text-4xl lg:text-6xl font-bold tracking-tight text-slate-900 dark:text-slate-50">
      Minimal & Clean Headline
    </h1>
    <p className="text-xl text-slate-600 dark:text-slate-400 max-w-2xl">
      Subheading with excellent readability
    </p>
    <Button className="bg-blue-600 hover:bg-blue-700">
      Get Started
    </Button>
  </div>
</section>

// Feature card
<Card className="border border-slate-200 dark:border-slate-800">
  <div className="p-3 bg-blue-50 dark:bg-blue-950/20 rounded-lg w-fit">
    <Icon className="text-blue-600" />
  </div>
  <h3 className="text-lg font-semibold">Feature Name</h3>
  <p className="text-slate-600 dark:text-slate-400">Description</p>
</Card>
```

---

## 2. Bold & Colorful

**Characteristics:**
- Vibrant color palette (multiple colors)
- Strong contrasts
- Playful illustrations/graphics
- Energetic, friendly tone
- Dynamic layouts
- Gradients and color overlays

**Best For:** B2C products, creative tools, modern brands, startups

**Example Brands:** Stripe (homepage), Figma, Framer, Webflow

---

### Color Palette

```javascript
{
  primary: {
    // Purple - creative, modern
    500: '#a855f7',
    600: '#9333ea',
    700: '#7c3aed',
  },
  secondary: {
    // Pink - energetic, friendly
    500: '#ec4899',
    600: '#db2777',
  },
  accent: {
    // Orange - warm, optimistic
    500: '#f59e0b',
    600: '#d97706',
  },
  gradients: {
    primary: 'linear-gradient(135deg, #7c3aed 0%, #ec4899 100%)',
    hero: 'linear-gradient(135deg, #667eea 0%, #764ba2 100%)',
  }
}
```

---

### Typography

```javascript
{
  fontFamily: {
    sans: ['Poppins', ...fontFamily.sans],  // Geometric, friendly
    // Alternative: Outfit
  },
  fontSize: {
    // Type scale: 1.333 (Perfect Fourth - more contrast)
    'xs': '0.75rem',
    'sm': '0.875rem',
    'base': '1rem',
    'lg': '1.333rem',
    'xl': '1.777rem',
    '2xl': '2.369rem',
    '3xl': '3.157rem',
    '4xl': '4.209rem',
    '5xl': '5.61rem',
    '6xl': '7.478rem',
  },
  fontWeight: {
    body: 400,
    medium: 500,
    semibold: 600,
    bold: 700,
    extrabold: 800,
  },
  lineHeight: {
    body: 1.6,  // More relaxed
    heading: 1.1,
  }
}
```

---

### Example Component Styles

```tsx
// Hero with gradient
<section className="bg-gradient-to-br from-purple-600 to-pink-600">
  <div className="container mx-auto px-4 py-24">
    <h1 className="text-5xl lg:text-7xl font-extrabold text-white">
      Bold Headline
    </h1>
    <Button className="bg-orange-500 hover:bg-orange-600 text-white">
      Get Started
    </Button>
  </div>
</section>

// Colorful card
<Card className="border-2 border-purple-200 bg-gradient-to-br from-purple-50 to-pink-50">
  <div className="p-4 bg-gradient-to-br from-purple-500 to-pink-500 rounded-xl w-fit">
    <Icon className="text-white" />
  </div>
  <h3 className="text-xl font-bold text-purple-900">Feature</h3>
</Card>
```

---

## 3. Glassmorphism (Premium Modern)

**Characteristics:**
- Translucent, frosted-glass backgrounds
- Layered depth
- Subtle borders and highlights
- backdrop-filter: blur()
- Sophisticated, premium feel
- Dark backgrounds with light overlays

**Best For:** High-end SaaS, design tools, innovative products, dashboards

**Example Brands:** Linear (dashboard), Arc Browser, Apple (web)

---

### Color Palette

```javascript
{
  primary: {
    500: '#3b82f6',  // Blue - trust + premium
    600: '#2563eb',
  },
  secondary: {
    500: '#8b5cf6',  // Purple - creative
    600: '#7c3aed',
  },
  accent: {
    500: '#06b6d4',  // Cyan - modern tech
    600: '#0891b2',
  },
  glass: {
    light: 'rgba(255, 255, 255, 0.1)',
    dark: 'rgba(0, 0, 0, 0.1)',
    border: 'rgba(255, 255, 255, 0.2)',
  }
}
```

---

### Glass Effect Utilities

```css
/* Add to globals.css */
.glass {
  background: rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(10px);
  -webkit-backdrop-filter: blur(10px);
  border: 1px solid rgba(255, 255, 255, 0.2);
  box-shadow: 0 8px 32px 0 rgba(31, 38, 135, 0.37);
}

.dark .glass {
  background: rgba(0, 0, 0, 0.2);
  border: 1px solid rgba(255, 255, 255, 0.1);
}

.glass-card {
  @apply glass rounded-xl;
}
```

---

### Example Component Styles

```tsx
// Hero with glass effect
<section className="relative min-h-screen bg-gradient-to-br from-slate-900 via-purple-900 to-slate-900">
  <div className="container mx-auto px-4 py-24">
    <div className="glass-card p-8 max-w-2xl">
      <h1 className="text-5xl font-bold text-white">
        Premium Product
      </h1>
      <p className="text-slate-300">
        Sophisticated description
      </p>
    </div>
  </div>
</section>

// Glass card
<div className="glass rounded-xl p-6 border border-white/20">
  <Icon className="text-cyan-400" />
  <h3 className="text-white font-semibold">Feature</h3>
  <p className="text-slate-300">Description</p>
</div>
```

**⚠️ Performance Warning:**
- Backdrop filters have performance cost
- Limit to max 5-7 glass elements per page
- Test on mobile devices
- Performance Optimizer must validate bundle size

---

## 4. Playful Minimalism

**Characteristics:**
- Clean but not sterile
- Subtle playful elements (rounded corners, soft shadows)
- Calm, friendly color palette
- Friendly but professional
- Balanced approach between minimal and fun

**Best For:** Productivity tools, collaboration apps, wellness apps, education

**Example Brands:** Notion, Framer, Cal.com, Pitch

---

### Color Palette

```javascript
{
  primary: {
    500: '#3b82f6',  // Warm blue
    600: '#2563eb',
  },
  secondary: {
    500: '#a78bfa',  // Soft purple
    600: '#8b5cf6',
  },
  accent: {
    500: '#fbbf24',  // Warm yellow - optimistic
    600: '#f59e0b',
  },
  neutral: 'warmGray',  // Warmer than stark gray
}
```

---

### Typography

```javascript
{
  fontFamily: {
    sans: ['Inter Variable', ...fontFamily.sans],
  },
  fontSize: {
    // Type scale: 1.25 (Major Third)
    // Same as Minimal & Clean
  },
  fontWeight: {
    body: 400,
    medium: 500,
    semibold: 600,  // Preferred for headings
    bold: 700,
  },
  letterSpacing: {
    normal: '0',
    wide: '0.01em',  // Slight tracking for friendliness
  }
}
```

---

### Spacing & Rounded Corners

```javascript
{
  borderRadius: {
    DEFAULT: '0.5rem',  // 8px - more rounded
    lg: '0.75rem',      // 12px
    xl: '1rem',         // 16px
    '2xl': '1.5rem',    // 24px - very rounded
  },
  boxShadow: {
    // Soft, friendly shadows
    sm: '0 1px 3px 0 rgb(0 0 0 / 0.08)',
    DEFAULT: '0 2px 8px 0 rgb(0 0 0 / 0.08)',
    lg: '0 4px 16px 0 rgb(0 0 0 / 0.08)',
  }
}
```

---

### Example Component Styles

```tsx
// Hero
<section className="bg-gradient-to-b from-blue-50 to-white dark:from-slate-900 dark:to-slate-800">
  <div className="container mx-auto px-4 py-24">
    <h1 className="text-5xl lg:text-6xl font-semibold text-slate-900 dark:text-slate-50">
      Friendly Headline
    </h1>
    <p className="text-lg text-slate-600 dark:text-slate-400">
      Calm, inviting description
    </p>
    <Button className="rounded-xl bg-blue-600 hover:bg-blue-700">
      Get Started
    </Button>
  </div>
</section>

// Playful card
<Card className="rounded-2xl shadow-lg border-none bg-white dark:bg-slate-800">
  <div className="p-4 bg-yellow-100 dark:bg-yellow-900/20 rounded-xl w-fit">
    <Icon className="text-yellow-600" />
  </div>
  <h3 className="text-lg font-semibold">Feature</h3>
  <p className="text-slate-600 dark:text-slate-400">Description</p>
</Card>
```

---

## Choosing the Right Aesthetic

### Decision Matrix

| Audience | Product Type | Recommended Aesthetic |
|----------|--------------|----------------------|
| B2B professionals | SaaS, tools, analytics | **Minimal & Clean** |
| Developers | Dev tools, APIs, CLIs | **Minimal & Clean** |
| Creatives | Design tools, creative software | **Bold & Colorful** |
| Consumers | B2C apps, games, social | **Bold & Colorful** |
| Premium/Enterprise | High-end SaaS, enterprise | **Glassmorphism** |
| Productivity users | Collaboration, productivity | **Playful Minimalism** |
| Wellness/Education | Health, learning, meditation | **Playful Minimalism** |

---

## Research-Backed Recommendations

**Minimal & Clean:**
- Increases conversion by 23% for B2B SaaS (source: Unbounce, 2024)
- 78% of top SaaS products use minimal aesthetics

**Bold & Colorful:**
- 35% higher engagement for B2C (source: HubSpot, 2024)
- Younger audiences (18-34) prefer vibrant designs

**Glassmorphism:**
- Premium perception increases by 41% (source: Nielsen Norman Group, 2024)
- Higher performance cost (benchmark before using)

**Playful Minimalism:**
- Balances professionalism (67% trust) with approachability (71% likability)
- Best for products requiring daily use (reduces fatigue)

---

**Last Updated:** November 17, 2025
