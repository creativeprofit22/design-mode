# DESIGN SYSTEMS 2025 - KNOWLEDGE BASE

**Last Updated:** November 2025

---

## Recommended: Shadcn UI + Radix UI

### Why Shadcn UI?

**Architecture:**
- Copy-paste components (NOT npm package)
- Built on Radix UI primitives (accessibility foundation)
- Styled with Tailwind CSS
- Full code ownership (modify anything)
- Zero dependency bloat

**Bundle Size:**
- Base: ~32KB (only Radix primitives you use)
- Per component: 2-8KB average
- Example landing page: ~45KB total
- vs Material UI: ~120KB minimum
- vs Chakra UI: ~95KB minimum

**Accessibility:**
- WCAG 2.2 AA compliant out of the box
- Keyboard navigation built-in
- Screen reader optimized
- ARIA attributes automatic

**Components Available:**
- Button, Card, Input, Select, Checkbox, Radio
- Dialog, Sheet, Dropdown Menu, Popover
- Table, Tabs, Accordion, Collapsible
- Toast, Alert Dialog, Context Menu
- Form components with validation
- Data table with sorting/filtering

**Setup:**

```bash
npx shadcn-ui@latest init
npx shadcn-ui@latest add button
npx shadcn-ui@latest add card
# Components copied to /components/ui/
```

**Usage:**

```typescript
import { Button } from "@/components/ui/button"
import { Card } from "@/components/ui/card"

<Button variant="default">Click me</Button>
<Card className="p-6">Content</Card>
```

---

## Alternative: Park UI

**When to Use:**
- Need pre-built patterns (faster setup)
- Willing to accept slightly larger bundle (~78KB)
- Want opinionated defaults

**Pros:**
- Pre-configured design patterns
- Faster initial setup
- Good documentation

**Cons:**
- Less flexible than Shadcn
- Smaller community
- Panda CSS instead of Tailwind (different ecosystem)

**Recommendation:** Use Shadcn UI unless user specifically requests Park UI

---

## Radix UI (Headless Primitives)

**What It Is:**
- Unstyled, accessible components
- Building blocks for custom design systems
- Used by Shadcn UI under the hood

**When to Use Directly:**
- Building completely custom design system
- Need maximum control
- Want lightest possible bundle

**Bundle:**
- ~32KB for commonly used primitives
- Import only what you need

**Example:**

```typescript
import * as Dialog from '@radix-ui/react-dialog'

<Dialog.Root>
  <Dialog.Trigger>Open</Dialog.Trigger>
  <Dialog.Portal>
    <Dialog.Overlay />
    <Dialog.Content>
      <Dialog.Title>Title</Dialog.Title>
      <Dialog.Description>Description</Dialog.Description>
    </Dialog.Content>
  </Dialog.Portal>
</Dialog.Root>
```

---

## Tailwind CSS v4

**What's New in v4:**
- 50% smaller CSS output
- Faster builds
- Better tree-shaking
- Native CSS variables
- Improved dark mode

**Setup:**

```bash
npm install tailwindcss@next
```

**Config:**

```javascript
// tailwind.config.ts
export default {
  darkMode: 'class',
  content: ['./app/**/*.{ts,tsx}', './components/**/*.{ts,tsx}'],
  theme: {
    extend: {
      colors: {
        primary: { /* color scale */ },
      }
    },
  },
}
```

**Key Features:**
- Utility-first (no CSS-in-JS runtime cost)
- Responsive modifiers (sm:, md:, lg:)
- Dark mode (dark:)
- Hover states (hover:, focus:)
- Custom design tokens

---

## Aceternity UI

**What It Is:**
- Collection of animated components
- Built on Framer Motion
- Cutting-edge visual effects

**When to Use:**
- Need standout animations
- Premium, modern aesthetic
- Willing to accept performance cost

**Bundle Impact:**
- +50-80KB per animated component
- Use sparingly (hero section only)

**Recommendation:** Use for inspiration, implement selectively with Framer Motion

---

## What NOT to Use (2025)

### Material UI ❌
- Too heavy (~120KB minimum)
- Dated aesthetic (2015-era design)
- CSS-in-JS runtime cost
- Hard to customize

### Ant Design ❌
- Very heavy (~180KB+)
- Chinese design language (not universal)
- Overcomplicated

### Chakra UI ❌
- Heavy (~95KB)
- CSS-in-JS runtime cost
- Slower builds than Tailwind

### Bootstrap ❌
- Outdated (2010s design)
- jQuery dependency (unnecessary)
- Class-based (not utility-first)

---

## Component Architecture Patterns

### Composition Pattern (Shadcn UI Approach):

```typescript
// Base UI primitive
import { Card, CardHeader, CardTitle, CardDescription, CardContent } from "@/components/ui/card"

// Composed feature component
export function FeatureCard({ icon: Icon, title, description }: FeatureCardProps) {
  return (
    <Card>
      <CardHeader>
        <Icon className="h-6 w-6 text-primary mb-2" />
        <CardTitle>{title}</CardTitle>
        <CardDescription>{description}</CardDescription>
      </CardHeader>
    </Card>
  )
}

// Usage
<FeatureCard
  icon={Zap}
  title="Lightning Fast"
  description="Optimized for speed"
/>
```

**Why This Works:**
- Shadcn primitives = accessibility
- Custom component = your design
- Tailwind = styling without CSS files
- TypeScript = type safety

---

## Design Token System

```typescript
// tailwind.config.ts
export default {
  theme: {
    extend: {
      colors: {
        border: "hsl(var(--border))",
        input: "hsl(var(--input))",
        ring: "hsl(var(--ring))",
        background: "hsl(var(--background))",
        foreground: "hsl(var(--foreground))",
        primary: {
          DEFAULT: "hsl(var(--primary))",
          foreground: "hsl(var(--primary-foreground))",
        },
        // ...more tokens
      },
    },
  },
}
```

```css
/* app/globals.css */
@layer base {
  :root {
    --background: 0 0% 100%;
    --foreground: 222.2 84% 4.9%;
    --primary: 221.2 83.2% 53.3%;
    --primary-foreground: 210 40% 98%;
    /* ...more variables */
  }

  .dark {
    --background: 222.2 84% 4.9%;
    --foreground: 210 40% 98%;
    /* ...dark mode values */
  }
}
```

---

## Best Practices

1. **Start with Shadcn UI** - Copy-paste, modify as needed
2. **Use Tailwind CSS** - Utility-first, no runtime cost
3. **Radix for Accessibility** - Built-in, no extra work
4. **Composition over Monolith** - Small, reusable pieces
5. **Type Everything** - TypeScript for props
6. **Dark Mode Native** - Use CSS variables
7. **Performance Budget** - < 90KB total bundle

---

## Quick Start Template

```bash
# 1. Initialize Next.js project
npx create-next-app@latest my-app --typescript --tailwind --app

# 2. Initialize Shadcn UI
npx shadcn-ui@latest init

# 3. Add essential components
npx shadcn-ui@latest add button card input

# 4. Start building
npm run dev
```

**Result:** Production-ready setup in < 5 minutes

---

**REMEMBER:** Shadcn UI + Tailwind v4 is the 2025 standard for lean, accessible, performant design systems. Unless user has specific needs, always recommend this stack.
