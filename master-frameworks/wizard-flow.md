# WIZARD FLOW FRAMEWORK

**Purpose:** Complete 4-step wizard flow for Design Mode (mirrors Supah Copy Mazta pattern)

---

## 4-Step Wizard Structure

```
Step 1: Template Selection
    ↓
Step 2: Requirements Gathering (Brief)
    ↓
Step 3: Style & Preferences Selection
    ↓
Step 4: Review & Confirmation
    ↓
EXECUTION: Route to specialist agents
```

---

## STEP 1: Template Selection

**Objective:** Quickly narrow scope, set expectations

**UI:**

```
Welcome to Design Mode! What would you like to design today?

SELECT A TEMPLATE:

📄 Landing Pages
  → SaaS Product Launch (Hero, Features, Pricing, Testimonials, CTA)
  → Lead Generation (Hero, Benefits, Form, Social Proof)
  → App Download (Hero, App Preview, Features, Download CTAs)

🎛️ Dashboards & Apps
  → Admin Dashboard (Sidebar, Stats, Tables, Charts)
  → Analytics Dashboard (Metrics, Graphs, Filters, Export)
  → SaaS Application (Multi-page app, settings, CRUD)

🧩 Component Libraries
  → Design System from Scratch (Full token system, docs)
  → Component Collection (Reusable components only)

💡 Not Sure? → Describe your project and I'll suggest a template
```

**User Actions:**
- Select template → Continue to Step 2
- Click "Not Sure" → Auto-research mode → Suggest template

**Auto-Research Trigger:**

```
USER: "I'm building a new product and need a website"

ORCHESTRATOR:
"Let me ask a few questions to suggest the right template:
- Is this a physical product or software?
- Who's the target customer?
- Main goal? (Generate leads / Sign ups / Sales)"

[User responds]

ORCHESTRATOR:
"Perfect! I recommend 'SaaS Product Launch' template.
This includes: Hero, Features, Pricing, Testimonials, CTA.
Proceed with this?"
```

**State After Step 1:**

```json
{
  "wizard_step": 1,
  "template": "saas-product-launch",
  "template_config": {
    "sections": ["hero", "features", "pricing", "testimonials", "cta", "footer"],
    "required_content": ["product_name", "tagline", "features", "pricing"],
    "optional_content": ["demo_video", "customer_logos"]
  }
}
```

---

## STEP 2: Requirements Gathering (Brief)

**Objective:** Collect all content, understand goals

**Dynamic Questions (Template-Specific):**

### For SaaS Product Launch:

```
Great! Let's gather the details for your SaaS landing page.

1. PRODUCT BASICS
Product Name: _______________
Tagline (1 sentence value prop): _______________

2. KEY FEATURES (3-5 features)
What makes your product valuable?

Feature 1:
  Name: _______________
  Description (1-2 sentences): _______________

[Add more] / [I need help brainstorming features]

3. TARGET AUDIENCE
Who is this for?
Primary user: _______________ (e.g., "Marketing managers at B2B companies")
User pain point: _______________ (e.g., "Struggle to prove marketing ROI")

4. PRICING (Optional)
[ ] Yes - I'll provide pricing tiers
[ ] No - Skip pricing section
[ ] Not sure - Show "Request Demo" instead

5. CALL-TO-ACTION
What's the primary action?
[ ] Start Free Trial
[ ] Schedule Demo
[ ] Custom: _______________
```

**Auto-Research Integration Points:**

1. **User needs help with features:**

```
USER: [Clicks "I need help brainstorming features"]

ORCHESTRATOR:
"I'll research common SaaS features for analytics products...

Based on successful analytics SaaS:
1. Real-time Dashboards - "See metrics update live"
2. AI-Powered Insights - "Automatic anomaly detection"
3. Custom Report Builder - "Drag-and-drop reports"
4. Team Collaboration - "Share dashboards, add comments"
5. API & Integrations - "Connect to 100+ data sources"

Which resonate? Or describe your features and I'll help refine."
```

2. **User provides vague pain point:**

```
USER: "Marketers need better data"

ORCHESTRATOR:
"Let me make this more specific and compelling.

More specific pain points:
- 'Spend 10+ hours weekly compiling reports from multiple tools'
- 'Can't prove which campaigns drive revenue'
- 'Leadership questions marketing budget ROI'

Which resonates most?"
```

**UX Researcher Validation (Automatic Background):**

After user completes Step 2:
```
[UX Researcher analyzes brief]

UX Researcher:
"Quick UX check:

✓ Product value prop is clear
✓ Target audience well-defined
⚠️ Feature descriptions could be more benefit-focused

Would you like me to reframe your features with benefit language?

Current: 'Real-time Dashboards - See your metrics update live'
Better: 'Real-time Dashboards - Make decisions on current data, not yesterday's numbers'

[Auto-apply] / [Keep original]"
```

**State After Step 2:**

```json
{
  "wizard_step": 2,
  "template": "saas-product-launch",
  "content": {
    "product_name": "Acme Analytics",
    "tagline": "Turn marketing data into boardroom-ready insights in seconds",
    "features": [
      {
        "name": "Real-time Dashboards",
        "description": "Make decisions on current data, not yesterday's numbers"
      }
    ],
    "target_audience": {
      "primary_user": "Marketing managers at B2B companies",
      "pain_point": "Spend 10+ hours weekly compiling reports"
    },
    "cta": "Start Free Trial",
    "include_pricing": true
  }
}
```

---

## STEP 3: Style & Preferences Selection

**Objective:** Define visual aesthetic

**UI:**

```
Excellent! Now let's define the visual style.

1. DESIGN AESTHETIC

Choose a style (or ask for recommendation):

▸ Minimal & Clean [Preview image]
  Best for: B2B SaaS, developer tools
  Vibe: Trustworthy, efficient, no-nonsense
  Examples: Vercel, Linear, Supabase

▸ Bold & Colorful [Preview image]
  Best for: B2C products, creative tools
  Vibe: Energetic, friendly, modern
  Examples: Stripe, Figma, Framer

▸ Glassmorphism [Preview image]
  Best for: High-end SaaS, design tools
  Vibe: Cutting-edge, premium, sophisticated
  Examples: Linear dashboard, Arc Browser

▸ Playful Minimalism [Preview image]
  Best for: Productivity tools, collaboration
  Vibe: Friendly, calm, delightful
  Examples: Notion, Framer, Cal.com

[Recommend based on my brief] / [More options]

2. COLOR PALETTE

[ ] Auto-generate from brand color: #______
[ ] Choose from preset palettes
[ ] I'll provide custom colors later

3. TYPOGRAPHY

[ ] Use default (Inter Variable)
[ ] Modern Geometric (Poppins, Outfit)
[ ] Custom: _______________

4. SPECIAL FEATURES (Optional)

[ ] Animations & micro-interactions (+28KB)
[ ] Dark mode support (+5KB)
[ ] 3D elements (requires custom assets)
```

**Auto-Research Integration:**

1. **User clicks "Recommend based on my brief":**

```
ORCHESTRATOR:
"Analyzing your product (B2B analytics for marketers)...

RECOMMENDATION: Minimal & Clean

WHY:
- Marketing managers value clarity and professionalism
- Minimal designs increase conversion by 23% for B2B SaaS
- Your competitors (HubSpot, Salesforce) use this successfully

COLORS: Modern Blues (#2563eb primary)
- Blue conveys trust and professionalism
- 60% of B2B SaaS use blue = data/intelligence association

TYPOGRAPHY: Inter Variable
- Excellent readability
- Modern
- Variable font = 1 file, all weights

SPECIAL FEATURES:
- Subtle animations: YES (delightful without distracting)
- Dark mode: YES (analytics users prefer dark mode)
- 3D elements: NO (unnecessary complexity)

Proceed with these recommendations?"
```

**UI Designer Activation (Automatic Background):**

After user completes Step 3:
```
[UI Designer generates design tokens and mockup options]

UI Designer:
"Based on your selections (Minimal & Clean, Modern Blues):

I've designed 2 layout variations:

OPTION A: Centered Hero with Screenshot
- Hero centered, large heading
- Screenshot right side
- Features in 3-column grid

OPTION B: Asymmetric Split
- Hero left (60%), video right (40%)
- Features in bento-box layout
- More modern, dynamic feel

Both use:
- Color: #2563eb primary
- Typography: Inter Variable
- Spacing: Tailwind default

Which layout?"
```

**State After Step 3:**

```json
{
  "wizard_step": 3,
  "template": "saas-product-launch",
  "content": { /* from Step 2 */ },
  "style": {
    "aesthetic": "minimal-clean",
    "color_palette": {
      "primary": "#2563eb",
      "secondary": "#7c3aed",
      "neutral": "slate"
    },
    "typography": {
      "font_family": "Inter Variable",
      "scale": 1.25
    },
    "features": {
      "animations": true,
      "dark_mode": true
    }
  },
  "layout": {
    "chosen_option": "A"
  }
}
```

---

## STEP 4: Review & Confirmation

**Objective:** Summary, validation, user approval

**UI:**

```
Perfect! Here's a complete summary:

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PROJECT SUMMARY
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

TEMPLATE: SaaS Product Launch Landing Page

CONTENT:
✓ Product: Acme Analytics
✓ Tagline: Turn marketing data into insights in seconds
✓ Features: 5 features with benefit-focused descriptions
✓ Target: Marketing managers at B2B companies
✓ CTA: Start Free Trial
✓ Pricing: 3-tier pricing table

DESIGN:
✓ Style: Minimal & Clean
✓ Colors: Modern Blues (#2563eb)
✓ Typography: Inter Variable
✓ Layout: Centered hero with screenshot
✓ Dark Mode: Enabled
✓ Animations: Subtle micro-interactions

TECH STACK:
✓ Framework: Next.js 15 + React 19
✓ Styling: Tailwind CSS v4 + Shadcn UI
✓ Animations: Framer Motion

────────────────────────────────────
VALIDATION RESULTS
────────────────────────────────────

[Running: Accessibility Specialist...]
✓ Accessibility: WCAG 2.2 AA compliant
  - Color contrast 4.8:1 (passing)
  - Keyboard accessible
  - Screen reader optimized

[Running: Performance Optimizer...]
✓ Performance: Exceeds targets
  - Est. bundle: 87KB gzipped (< 90KB ✓)
  - Est. LCP: 1.8s (< 2.0s ✓)
  - Lazy loading enabled

────────────────────────────────────

Everything look good?

[✓ Yes, generate my design!]
[⚠️ Change something] → Which step? (1, 2, 3)
```

**Validation Checks (Automatic):**

1. **Accessibility Specialist** validates:
   - Color contrast ratios
   - Semantic HTML structure
   - ARIA requirements

2. **Performance Optimizer** estimates:
   - Bundle size
   - Core Web Vitals
   - Load time projections

**If Issues Found:**

```
⚠️ Accessibility Warning:
Primary CTA color (#3b82f6) on white = 4.2:1 contrast.
WCAG AA requires 4.5:1 minimum.

FIX: Darken to #2563eb (4.8:1 contrast ✓)

[Auto-fix and continue] / [Keep current color]
```

**State After Step 4:**

```json
{
  "wizard_step": 4,
  "confirmed": true,
  "validation": {
    "accessibility": { "passed": true, "fixes_applied": ["primary_color_darkened"] },
    "performance": { "estimated_bundle": "87KB", "meets_targets": true }
  },
  "ready_for_execution": true
}
```

---

## EXECUTION Phase

**After User Confirms:**

```
Orchestrator coordinates:
1. Component Builder → Generate all React components
2. Layout Architect → Create page structure
3. UI Designer → Finalize Tailwind config
4. Animation Designer → Add animations
5. Accessibility Specialist → Final audit
6. Performance Optimizer → Optimize assets

[Progress indicator shown to user]

DONE! Your design is ready! 🎉

Generated:
- 12 React components
- Responsive layout
- Dark mode support
- WCAG AA accessible
- 85KB bundle (optimized)

[Download ZIP] / [View Files] / [Deploy to Vercel]
```

---

**REMEMBER:** This wizard flow is mandatory for all new design projects. It ensures complete requirements gathering, reduces back-and-forth, and produces quality output on first try.
