# Component Patterns

**Purpose:** React/TypeScript component examples and patterns for common UI elements

---

## Pattern 1: Hero Component

```tsx
import { Button } from "@/components/ui/button"
import Image from "next/image"
import { motion } from "framer-motion"

interface HeroProps {
  heading: string
  subheading: string
  ctaText: string
  ctaHref: string
  imageSrc: string
}

export function Hero({
  heading,
  subheading,
  ctaText,
  ctaHref,
  imageSrc
}: HeroProps) {
  return (
    <section className="container mx-auto px-4 py-16 lg:py-24">
      <div className="max-w-4xl mx-auto text-center space-y-6">
        <motion.h1
          className="text-4xl lg:text-6xl font-bold tracking-tight"
          initial={{ opacity: 0, y: 20 }}
          animate={{ opacity: 1, y: 0 }}
          transition={{ duration: 0.5 }}
        >
          {heading}
        </motion.h1>

        <motion.p
          className="text-xl text-muted-foreground max-w-2xl mx-auto"
          initial={{ opacity: 0, y: 20 }}
          animate={{ opacity: 1, y: 0 }}
          transition={{ duration: 0.5, delay: 0.1 }}
        >
          {subheading}
        </motion.p>

        <motion.div
          initial={{ opacity: 0, y: 20 }}
          animate={{ opacity: 1, y: 0 }}
          transition={{ duration: 0.5, delay: 0.2 }}
        >
          <Button size="lg" asChild>
            <a href={ctaHref}>{ctaText}</a>
          </Button>
        </motion.div>

        <motion.div
          className="mt-12"
          initial={{ opacity: 0, y: 40 }}
          animate={{ opacity: 1, y: 0 }}
          transition={{ duration: 0.6, delay: 0.3 }}
        >
          <Image
            src={imageSrc}
            alt=""
            width={1200}
            height={800}
            priority
            className="rounded-lg shadow-2xl"
          />
        </motion.div>
      </div>
    </section>
  )
}
```

---

## Pattern 2: Feature Card

```tsx
import { Card, CardHeader, CardTitle, CardDescription } from "@/components/ui/card"
import { LucideIcon } from "lucide-react"

interface FeatureCardProps {
  icon: LucideIcon
  title: string
  description: string
}

export function FeatureCard({ icon: Icon, title, description }: FeatureCardProps) {
  return (
    <Card className="h-full">
      <CardHeader>
        <div className="mb-4 p-3 bg-primary/10 dark:bg-primary/20 rounded-lg w-fit">
          <Icon className="h-6 w-6 text-primary" />
        </div>
        <CardTitle>{title}</CardTitle>
        <CardDescription>{description}</CardDescription>
      </CardHeader>
    </Card>
  )
}
```

---

## Pattern 3: Pricing Card

```tsx
import { Button } from "@/components/ui/button"
import { Card, CardHeader, CardTitle, CardContent, CardFooter } from "@/components/ui/card"
import { Check } from "lucide-react"

interface PricingCardProps {
  tier: string
  price: number
  features: string[]
  ctaText: string
  ctaHref: string
  popular?: boolean
}

export function PricingCard({
  tier,
  price,
  features,
  ctaText,
  ctaHref,
  popular = false
}: PricingCardProps) {
  return (
    <Card className={popular ? "border-primary shadow-lg" : ""}>
      <CardHeader>
        {popular && (
          <span className="text-xs font-semibold text-primary uppercase tracking-wider">
            Most Popular
          </span>
        )}
        <CardTitle className="text-2xl">{tier}</CardTitle>
        <div className="mt-4">
          <span className="text-4xl font-bold">${price}</span>
          <span className="text-base font-normal text-muted-foreground">/month</span>
        </div>
      </CardHeader>

      <CardContent className="space-y-4">
        <ul className="space-y-3">
          {features.map((feature) => (
            <li key={feature} className="flex items-start gap-2">
              <Check className="h-5 w-5 text-green-600 dark:text-green-500 shrink-0 mt-0.5" />
              <span className="text-sm">{feature}</span>
            </li>
          ))}
        </ul>
      </CardContent>

      <CardFooter>
        <Button
          className="w-full"
          variant={popular ? "default" : "outline"}
          asChild
        >
          <a href={ctaHref}>{ctaText}</a>
        </Button>
      </CardFooter>
    </Card>
  )
}
```

---

## Pattern 4: Form with Validation

```tsx
"use client"

import { Button } from "@/components/ui/button"
import { Input } from "@/components/ui/input"
import { Label } from "@/components/ui/label"
import { useForm } from "react-hook-form"
import { zodResolver } from "@hookform/resolvers/zod"
import { z } from "zod"

const formSchema = z.object({
  name: z.string().min(1, "Name is required"),
  email: z.string().email("Invalid email address"),
  company: z.string().optional(),
})

type FormData = z.infer<typeof formSchema>

interface LeadFormProps {
  onSubmit: (data: FormData) => void
}

export function LeadForm({ onSubmit }: LeadFormProps) {
  const {
    register,
    handleSubmit,
    formState: { errors, isSubmitting },
  } = useForm<FormData>({
    resolver: zodResolver(formSchema),
  })

  return (
    <form onSubmit={handleSubmit(onSubmit)} className="space-y-4">
      <div className="space-y-2">
        <Label htmlFor="name">Name *</Label>
        <Input
          id="name"
          {...register("name")}
          aria-invalid={errors.name ? "true" : "false"}
          aria-describedby={errors.name ? "name-error" : undefined}
        />
        {errors.name && (
          <p id="name-error" className="text-sm text-red-600" role="alert">
            {errors.name.message}
          </p>
        )}
      </div>

      <div className="space-y-2">
        <Label htmlFor="email">Email *</Label>
        <Input
          id="email"
          type="email"
          {...register("email")}
          aria-invalid={errors.email ? "true" : "false"}
          aria-describedby={errors.email ? "email-error" : undefined}
        />
        {errors.email && (
          <p id="email-error" className="text-sm text-red-600" role="alert">
            {errors.email.message}
          </p>
        )}
      </div>

      <div className="space-y-2">
        <Label htmlFor="company">Company</Label>
        <Input id="company" {...register("company")} />
      </div>

      <Button type="submit" className="w-full" disabled={isSubmitting}>
        {isSubmitting ? "Submitting..." : "Get Started"}
      </Button>
    </form>
  )
}
```

---

## Pattern 5: Responsive Grid Layouts

```tsx
// 3-column responsive grid (Features)
<div className="
  grid
  grid-cols-1           // Mobile: stacked
  md:grid-cols-2        // Tablet: 2 columns
  lg:grid-cols-3        // Desktop: 3 columns
  gap-6 lg:gap-12
">
  {features.map(feature => <FeatureCard {...feature} />)}
</div>

// 2-column with asymmetric split (Hero)
<div className="
  grid
  grid-cols-1           // Mobile: stacked
  lg:grid-cols-2        // Desktop: 2 columns
  gap-12
  items-center
">
  <div>{/* Content */}</div>
  <div>{/* Image */}</div>
</div>

// Centered content with max-width
<div className="max-w-4xl mx-auto text-center space-y-6">
  {/* Centered content */}
</div>
```

---

## Pattern 6: Dark Mode Styling

```tsx
// Background and text colors
<div className="
  bg-white dark:bg-slate-900
  text-slate-900 dark:text-slate-50
">
  Content
</div>

// Border colors
<div className="
  border border-slate-200 dark:border-slate-800
">
  Content
</div>

// Muted text
<p className="text-muted-foreground">
  Subheading text
</p>

// Icon containers
<div className="bg-primary/10 dark:bg-primary/20">
  <Icon className="text-primary" />
</div>
```

---

## Pattern 7: Semantic HTML Structure

```tsx
// Landing page structure
<>
  <header>
    <nav aria-label="Main navigation">
      {/* Navigation */}
    </nav>
  </header>

  <main id="main">
    <section aria-labelledby="hero-heading">
      <h1 id="hero-heading">Page Title</h1>
    </section>

    <section aria-labelledby="features-heading">
      <h2 id="features-heading">Features</h2>
      {/* Features */}
    </section>

    <section aria-labelledby="pricing-heading">
      <h2 id="pricing-heading">Pricing</h2>
      {/* Pricing */}
    </section>
  </main>

  <footer>
    <nav aria-label="Footer navigation">
      {/* Footer links */}
    </nav>
  </footer>
</>
```

---

## Composition Pattern

**Key Principle:** Build components FROM Shadcn UI primitives, not from scratch.

```tsx
// ✅ GOOD: Compose from primitives
import { Button } from "@/components/ui/button"
import { Card } from "@/components/ui/card"

export function PricingCard() {
  return (
    <Card>
      <Button>Subscribe</Button>
    </Card>
  )
}

// ❌ BAD: Reinvent the wheel
export function PricingCard() {
  return (
    <div className="rounded-lg border bg-white p-6">
      <button className="rounded-md bg-blue-600 px-4 py-2">
        Subscribe
      </button>
    </div>
  )
}
```

---

**Last Updated:** November 17, 2025
