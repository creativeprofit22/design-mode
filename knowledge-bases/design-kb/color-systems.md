# Color Systems

**Purpose:** Color palette generation, accessibility validation, and dark mode implementation

---

## Color Scale Generation

### Full Tailwind Scale (50-950)

Every primary, secondary, and accent color should have a full 11-step scale:

```javascript
{
  50: '#....',   // Lightest (backgrounds, hover states)
  100: '#....',
  200: '#....',
  300: '#....',
  400: '#....',
  500: '#....',  // Base color
  600: '#....',  // Default primary (most contrast on white)
  700: '#....',
  800: '#....',
  900: '#....',
  950: '#....',  // Darkest (text on light backgrounds)
}
```

---

### How to Generate a Scale

**Option 1: Use Tailwind's Color Generator**
- Visit: https://uicolors.app/create
- Input base color (e.g., #2563eb)
- Generate full scale automatically
- Export as Tailwind config

**Option 2: Manual Calculation** (if tool unavailable)
```
Base color: #2563eb (Blue 600)

Lighter shades (50-500):
- Increase lightness by 10-15% per step
- Decrease saturation slightly (2-3% per step)

Darker shades (700-950):
- Decrease lightness by 10-15% per step
- Increase saturation slightly (2-3% per step)
```

---

## Accessibility: Color Contrast Validation

### WCAG 2.2 AA Requirements

```
Normal text (< 24px):       ≥ 4.5:1 contrast ratio
Large text (≥ 24px):        ≥ 3:1 contrast ratio
UI components (buttons):    ≥ 3:1 contrast ratio
Graphical objects:          ≥ 3:1 contrast ratio
```

---

### Common Combinations to Test

**Primary color on white:**
```javascript
// Test all shades until you find passing contrast
testContrast('#eff6ff', '#ffffff')  // 50 on white = ❌ Fails
testContrast('#2563eb', '#ffffff')  // 600 on white = ✅ 4.8:1 (PASS)
testContrast('#1e3a8a', '#ffffff')  // 900 on white = ✅ 8.2:1 (PASS)
```

**White text on primary color:**
```javascript
testContrast('#ffffff', '#3b82f6')  // white on 500 = ❌ 3.2:1 (FAIL for text)
testContrast('#ffffff', '#2563eb')  // white on 600 = ✅ 4.8:1 (PASS)
testContrast('#ffffff', '#1d4ed8')  // white on 700 = ✅ 6.1:1 (PASS)
```

**Recommended defaults:**
- Primary text on white: Use 600 or darker
- White text on primary: Use 600 or darker
- Buttons: 600 background, white text

---

### Contrast Checker Tool

Use WebAIM Contrast Checker:
- URL: https://webaim.org/resources/contrastchecker/
- Input foreground and background colors
- See if it passes WCAG AA

---

## Dark Mode Color Strategy

### DON'T Just Invert Colors

```tsx
// ❌ BAD: Simple inversion
<div className="bg-white dark:bg-black">
  <p className="text-black dark:text-white">Text</p>
</div>
```

**Problems:**
- Pure black (#000) is too harsh on eyes
- Pure white (#fff) on black = too much contrast
- Doesn't account for color perception differences

---

### DO: Use Adjusted Colors

```tsx
// ✅ GOOD: Adjusted dark mode colors
<div className="
  bg-white dark:bg-slate-950
  text-slate-900 dark:text-slate-50
">
  Content
</div>
```

**Why:**
- slate-950 (#020617) = soft black (easier on eyes)
- slate-50 (#f8fafc) = slightly off-white (better contrast)
- Maintains WCAG AA ratios in both modes

---

### Dark Mode Color Adjustments

**Backgrounds:**
```javascript
// Light mode
bg-white           // #ffffff

// Dark mode (replace with)
dark:bg-slate-950  // #020617 (soft black)
dark:bg-slate-900  // #0f172a (elevated surfaces)
dark:bg-slate-800  // #1e293b (cards, higher elevation)
```

**Text:**
```javascript
// Light mode
text-slate-900     // #0f172a (almost black)
text-slate-600     // #475569 (muted)

// Dark mode
dark:text-slate-50   // #f8fafc (primary text)
dark:text-slate-400  // #94a3b8 (muted text)
```

**Borders:**
```javascript
// Light mode
border-slate-200   // #e2e8f0

// Dark mode
dark:border-slate-800  // #1e293b
```

**Primary color adjustments:**
```javascript
// Light mode
bg-blue-600        // #2563eb

// Dark mode (use lighter shade for better contrast)
dark:bg-blue-500   // #3b82f6
```

---

## Semantic Color Naming

### Use Tailwind's Semantic Colors

```javascript
// tailwind.config.ts
export default {
  theme: {
    extend: {
      colors: {
        // Instead of just primary/secondary, use semantic names
        background: 'hsl(var(--background))',
        foreground: 'hsl(var(--foreground))',
        card: 'hsl(var(--card))',
        'card-foreground': 'hsl(var(--card-foreground))',
        popover: 'hsl(var(--popover))',
        'popover-foreground': 'hsl(var(--popover-foreground))',
        primary: 'hsl(var(--primary))',
        'primary-foreground': 'hsl(var(--primary-foreground))',
        secondary: 'hsl(var(--secondary))',
        'secondary-foreground': 'hsl(var(--secondary-foreground))',
        muted: 'hsl(var(--muted))',
        'muted-foreground': 'hsl(var(--muted-foreground))',
        accent: 'hsl(var(--accent))',
        'accent-foreground': 'hsl(var(--accent-foreground))',
        destructive: 'hsl(var(--destructive))',
        'destructive-foreground': 'hsl(var(--destructive-foreground))',
        border: 'hsl(var(--border))',
        input: 'hsl(var(--input))',
        ring: 'hsl(var(--ring))',
      }
    }
  }
}
```

**Then in globals.css:**
```css
@layer base {
  :root {
    --background: 0 0% 100%;
    --foreground: 222.2 84% 4.9%;
    --primary: 221.2 83.2% 53.3%;
    --primary-foreground: 210 40% 98%;
    /* ... */
  }

  .dark {
    --background: 222.2 84% 4.9%;
    --foreground: 210 40% 98%;
    --primary: 217.2 91.2% 59.8%;
    --primary-foreground: 222.2 47.4% 11.2%;
    /* ... */
  }
}
```

**Usage:**
```tsx
<div className="bg-background text-foreground">
  <Card className="bg-card text-card-foreground">
    <p className="text-muted-foreground">Muted text</p>
  </Card>
</div>
```

**Benefits:**
- Automatic dark mode switching
- Consistent across components
- Easy to rebrand (change CSS variables only)

---

## Color Palette Size

### Minimal & Clean
```javascript
{
  primary: { ...fullScale },  // Blue
  neutral: 'slate',           // Built-in Tailwind scale
  accent: '#6366f1',          // Single accent (indigo)
}
```

### Bold & Colorful
```javascript
{
  primary: { ...fullScale },    // Purple
  secondary: { ...fullScale },  // Pink
  accent: { ...fullScale },     // Orange
  neutral: 'warmGray',
}
```

### Glassmorphism
```javascript
{
  primary: { ...fullScale },    // Blue
  secondary: { ...fullScale },  // Purple
  accent: '#06b6d4',            // Cyan
  neutral: 'coolGray',
  glass: {  // Special alpha values
    light: 'rgba(255, 255, 255, 0.1)',
    border: 'rgba(255, 255, 255, 0.2)',
  }
}
```

---

## Industry-Specific Color Recommendations

### Finance & Banking
- **Primary:** Blue (#2563eb) - Trust, stability
- **Secondary:** Green (#10b981) - Growth, money
- **Avoid:** Red (associated with debt/danger)

### Healthcare
- **Primary:** Blue (#3b82f6) - Trust, calm
- **Secondary:** Green (#22c55e) - Health, wellness
- **Accent:** Teal (#14b8a6) - Clean, professional

### Technology & SaaS
- **Primary:** Blue (#2563eb) - Intelligence, tech
- **Secondary:** Purple (#7c3aed) - Innovation
- **Accent:** Cyan (#06b6d4) - Modern

### Creative & Design Tools
- **Primary:** Purple (#a855f7) - Creativity
- **Secondary:** Pink (#ec4899) - Energy
- **Accent:** Orange (#f59e0b) - Enthusiasm

### E-commerce & Retail
- **Primary:** Orange (#f97316) - Urgency, CTA
- **Secondary:** Blue (#3b82f6) - Trust
- **Accent:** Green (#22c55e) - Success, purchase

---

## Color Psychology Quick Reference

| Color | Emotions | Best For |
|-------|----------|----------|
| Blue | Trust, calm, professional | B2B, finance, healthcare, tech |
| Purple | Creative, innovative, premium | Design tools, luxury, creative |
| Green | Growth, health, success | Finance, health, sustainability |
| Orange | Energetic, friendly, urgent | E-commerce, B2C, calls-to-action |
| Pink | Playful, modern, approachable | B2C, fashion, beauty, creative |
| Red | Urgency, passion, danger | Use sparingly (alerts, sales) |
| Yellow | Optimistic, attention-grabbing | Accents only (too harsh as primary) |
| Teal | Balanced, professional, calm | Healthcare, wellness, corporate |

---

## Validation Checklist

Before finalizing color palette:

- ✅ Full scale generated (50-950) for all non-neutral colors
- ✅ Primary on white ≥ 4.5:1 contrast (text)
- ✅ White on primary ≥ 4.5:1 contrast (text)
- ✅ UI components ≥ 3:1 contrast
- ✅ Dark mode colors adjusted (not inverted)
- ✅ Dark mode contrast validated (all combinations)
- ✅ Semantic color names used (background, foreground, muted)
- ✅ Industry-appropriate color choices
- ✅ Color psychology aligns with brand

---

**Last Updated:** November 17, 2025
