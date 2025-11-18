# DESIGN MODE - Complete Architecture

**Status:** ✅ Complete Specification (Ready for Implementation)
**Created:** November 17, 2025
**Pattern:** Mirrors Supah Copy Mazta (4-step wizard, auto-research, specialist agents)

---

## 🎯 What This Is

Design Mode is a wizard-based design system generator for Agent Girl that creates production-ready, accessible, performant React applications. It follows the exact pattern of Supah Copy Mazta but for design instead of copywriting.

**Key Features:**
- 4-step wizard flow (Template → Brief → Style → Review)
- Auto-research when user lacks information
- 6 specialist agents (Component Builder, UI Designer, etc.)
- Complete mode isolation (no contamination from other modes)
- Production-ready output (WCAG AA, < 90KB bundle, LCP < 2.0s)

---

## 📁 File Structure

```
/design-mode/
├── orchestrator.md                          (Master router, wizard flow)
│
├── /agents/                                 (6 specialist agents)
│   ├── component-builder.md                (React/TypeScript component generation)
│   ├── ui-designer.md                      (Design tokens, style specs)
│   ├── accessibility-specialist.md         (WCAG 2.2 AA validation)
│   ├── performance-optimizer.md            (Bundle optimization, Core Web Vitals)
│   ├── ux-researcher.md                    (Requirements validation, UX patterns)
│   ├── layout-architect.md                 (Grid systems, responsive layouts)
│   └── animation-designer.md               (Framer Motion micro-interactions)
│
├── /knowledge-bases/                        (Design expertise)
│   ├── design-systems-2025.md              (Shadcn UI, Radix, Tailwind v4)
│   ├── ui-patterns-2025.md                 (Latest design trends)
│   ├── wcag-2-2-standards.md               (Accessibility requirements)
│   ├── performance-budgets.md              (Bundle targets, Core Web Vitals)
│   └── ...
│
├── /master-frameworks/                      (Core workflows)
│   ├── wizard-flow.md                      (Complete 4-step wizard specification)
│   ├── component-architecture.md           (React composition patterns)
│   └── responsive-strategy.md              (Mobile-first design)
│
├── /quality-checklists/                     (Validation gates)
│   ├── design-quality-checklist.md         (Final validation before delivery)
│   ├── accessibility-checklist.md          (WCAG compliance)
│   └── performance-checklist.md            (Bundle, speed, Core Web Vitals)
│
├── /templates/                              (Template configurations)
│   ├── saas-product-launch.json            (SaaS landing page template)
│   ├── lead-generation.json                (Lead gen page template)
│   ├── admin-dashboard.json                (Dashboard template)
│   └── ...
│
└── README.md                                (This file)
```

---

## 🔧 How It Works

### 1. User Starts Design Mode

```
User: "I need to design a landing page"

Orchestrator: Detects new project → Starts wizard flow
```

### 2. Wizard Flow (4 Steps)

**Step 1: Template Selection**
- User selects from 6 templates OR describes project
- If describes → Auto-research suggests template

**Step 2: Requirements Gathering**
- Template-specific questions (product name, features, target audience, etc.)
- If user says "I don't know" → Auto-research provides options
- UX Researcher validates completeness (background)

**Step 3: Style & Preferences**
- User selects aesthetic (Minimal, Bold, Glassmorphism, Playful)
- Color palette, typography, special features
- If user asks "What should I choose?" → Auto-research recommends based on brief
- UI Designer generates mockup options (background)

**Step 4: Review & Confirmation**
- Complete summary shown
- Accessibility Specialist validates colors (background)
- Performance Optimizer estimates bundle size (background)
- User confirms OR requests changes

### 3. Execution (After Confirmation)

Orchestrator coordinates specialist agents in sequence:

```
1. Component Builder → Generate React components
2. Layout Architect → Create page structure
3. UI Designer → Finalize Tailwind config
4. Animation Designer → Add micro-interactions (if enabled)
5. Accessibility Specialist → Final WCAG audit
6. Performance Optimizer → Bundle optimization

Output: Complete Next.js 15 app, ready to deploy
```

---

## 🎨 Tech Stack (Recommended)

**Design System:** Shadcn UI + Radix UI
- **Why:** Copy-paste components, ~32KB base, WCAG AA built-in

**Styling:** Tailwind CSS v4
- **Why:** 50% smaller CSS, utility-first, no runtime cost

**Framework:** Next.js 15 + React 19
- **Why:** Server components, excellent performance, Edge deployment

**Animation:** Framer Motion
- **Why:** Best React DX, 28KB, GPU-accelerated

**Type Safety:** TypeScript (strict mode)

---

## 📊 Quality Standards

### Accessibility (WCAG 2.2 AA) - MANDATORY
- ✅ Color contrast ≥ 4.5:1 (text), ≥ 3:1 (UI)
- ✅ Semantic HTML, proper heading hierarchy
- ✅ Keyboard navigation, focus indicators
- ✅ Screen reader optimized
- ✅ ARIA attributes where needed

### Performance - MANDATORY
- ✅ Bundle size < 90KB gzipped (MVP)
- ✅ LCP < 2.0s (excellent)
- ✅ FID < 50ms (excellent)
- ✅ CLS < 0.05 (excellent)
- ✅ Lighthouse Performance score > 95

### Code Quality - MANDATORY
- ✅ TypeScript (no `any`)
- ✅ Functional components with hooks
- ✅ Props typed with interfaces
- ✅ Tailwind CSS (no inline styles)
- ✅ Production-ready code

---

## 🚀 Templates

### MVP (Phase 1):
1. **SaaS Product Launch** - Hero, Features, Pricing, Testimonials, CTA
2. **Lead Generation** - Hero, Benefits, Form, Social Proof
3. **App Download** - Hero, App Preview, Features, Download CTAs

### Phase 2:
4. **Admin Dashboard** - Sidebar, Stats, Tables, Charts
5. **Analytics Dashboard** - Metrics, Graphs, Filters
6. **Multi-page Website** - Homepage, About, Services, Contact

---

## 🧠 Specialist Agents

### 1. Component Builder
- Generates production-ready React/TypeScript components
- Uses Shadcn UI primitives
- Ensures accessibility, performance

### 2. UI Designer
- Creates design tokens (colors, typography, spacing)
- Generates Tailwind config
- Provides layout mockup options

### 3. Accessibility Specialist
- Validates WCAG 2.2 AA compliance
- Checks color contrast, semantic HTML, keyboard navigation
- Auto-fixes common issues

### 4. Performance Optimizer
- Enforces bundle budgets (< 90KB)
- Optimizes images (WebP, AVIF)
- Code splitting, tree shaking

### 5. UX Researcher
- Validates requirements completeness
- Researches UX best practices
- Provides data-driven recommendations

### 6. Layout Architect
- Designs responsive grid systems
- Creates page structures
- Handles complex layouts (dashboards)

### 7. Animation Designer (Phase 2)
- Adds subtle micro-interactions
- Uses Framer Motion
- Maintains performance budget

---

## 🔍 Auto-Research Mode

**Triggers:**
- User says "I don't know"
- User asks "What's popular?" or "What should I use?"
- Orchestrator detects knowledge gap
- User provides vague requirements

**Process:**
1. Trigger detection
2. WebSearch for relevant information
3. Synthesize findings
4. Present options with rationale
5. User validates/selects
6. Continue wizard flow

**Example:**
```
User: "What design system should I use for a dashboard?"

Orchestrator:
[Triggers WebSearch]
"Based on research, here are the top options:

1. Shadcn UI + Tailwind (Recommended)
   - Bundle: ~45KB
   - Pros: Full control, excellent performance
   - Used by: Vercel, Linear

2. Park UI
   - Bundle: ~78KB
   - Pros: Pre-built patterns, faster setup

For dashboards, I recommend Shadcn UI. Proceed?"
```

---

## 🔒 Mode Isolation

**Forbidden References:**
- Copywriting frameworks (Gary Halbert, Dan Kennedy, Russell Brunson, etc.)
- Marketing copy swipe files
- Supah Copy Mazta knowledge bases

**Why:** Design Mode generates DESIGN (layouts, components, styles), NOT marketing copy content.

**Exception:** Phase 2 - MCP protocol can request copy from Supah Copy Mazta mode for integrated workflows.

---

## 📈 Performance Budgets

| Metric | MVP Target | Phase 2 Target |
|--------|------------|----------------|
| Bundle Size | < 90KB gzipped | < 130KB gzipped |
| LCP | < 2.0s | < 2.5s |
| FID | < 50ms | < 100ms |
| CLS | < 0.05 | < 0.1 |
| Lighthouse | > 95 | > 90 |

---

## 🎯 Implementation Roadmap

### Phase 1: MVP (6 weeks)
- 4-step wizard complete
- 3 templates (SaaS, Lead Gen, App Download)
- 3 core agents (Component Builder, UI Designer, Accessibility)
- Responsive, dark mode, SEO
- Bundle: ~90KB, LCP < 2.0s

### Phase 2: Intelligence (6 weeks)
- Auto-research system
- Animations (Framer Motion)
- MCP integration (copy from Supah Copy Mazta)
- +3 agents (UX Researcher, Performance Optimizer, Animation Designer)
- Bundle: ~130KB

### Phase 3: Advanced (8 weeks, optional)
- Compliance Specialist (HIPAA/GDPR) - if demand exists
- Form Builder with validation
- 3D elements (experimental)
- Bundle: 180KB-450KB (user opt-in)

---

## 🔑 Key Principles

1. **Wizard First:** New projects MUST go through wizard (ensures quality)
2. **Auto-Research:** System researches when user lacks knowledge
3. **Mode Isolation:** Zero contamination from copywriting modes
4. **Accessibility First:** WCAG 2.2 AA is non-negotiable
5. **Performance Budget:** < 90KB MVP, enforced by agents
6. **Lean & Mean:** No bloat, maximum performance
7. **Production-Ready:** Code ships immediately, no cleanup needed

---

## ✅ What's Been Built

**Complete Specifications:**
- ✅ Orchestrator (master router, wizard flow)
- ✅ 6 Specialist Agents (detailed prompts)
- ✅ Knowledge Bases (design systems, accessibility, performance)
- ✅ Master Frameworks (wizard flow, component architecture)
- ✅ Quality Checklists (accessibility, performance, code quality)
- ✅ Template Configurations (SaaS Product Launch complete)

**Status:** Ready for implementation in Agent Girl

---

## 🚧 Next Steps (Implementation)

1. **Integrate into Agent Girl:**
   - Register Design Mode in modes.ts
   - Add 6 specialist agents to agents.ts
   - Create UI wizard component (mirror Supah Copy Mazta wizard)

2. **Test Wizard Flow:**
   - Walk through all 4 steps
   - Validate auto-research triggers
   - Ensure agent coordination works

3. **Generate First Design:**
   - Test SaaS Product Launch template
   - Validate output quality (accessibility, performance, code)
   - Iterate based on findings

4. **User Testing:**
   - Get feedback on wizard UX
   - Validate output meets user needs
   - Refine based on real-world usage

---

## 📚 Learning from Supah Copy Mazta

**Patterns Applied:**
- ✅ 4-step wizard (Template → Brief → Voice/Tone → Review)
- ✅ Auto-research triggers ("I don't know" → system researches)
- ✅ Specialist agent coordination
- ✅ Mode isolation enforcement
- ✅ Quality-first approach (validation before delivery)
- ✅ Progressive disclosure (step-by-step guidance)

**Design Mode Differences:**
- Design vs Copy (layouts vs marketing copy)
- 6 agents vs 4 agents (more specialized for design)
- Visual output (code + styles) vs Text output (copy)
- Performance budgets (bundle size critical for design)

---

## 🎓 For Future Developers

**This architecture is:**
- Modular (agents are independent)
- Extensible (add new templates easily)
- Quality-focused (mandatory validation gates)
- User-friendly (wizard handles complexity)
- Production-ready (outputs deploy immediately)

**When adding new features:**
1. Follow the wizard pattern (guide users step-by-step)
2. Add auto-research for knowledge gaps
3. Create specialist agent if needed
4. Update quality checklists
5. Validate performance impact

---

**Last Updated:** November 17, 2025
**Version:** 1.0
**Status:** ✅ Complete Architectural Specification
**Next Phase:** Implementation in Agent Girl
