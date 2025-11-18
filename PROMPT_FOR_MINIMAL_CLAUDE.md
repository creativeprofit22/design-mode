# Design Mode Implementation Prompt for Minimal Claude

**Date:** November 18, 2025
**GitHub Repository:** https://github.com/creativeprofit22/design-mode
**Local Path:** `/home/reaver47/Documents/agent-girl/designer-mode/design-mode/`

---

## Task Overview

Build **Design Mode** - a wizard-based design system generator that creates production-ready, accessible, performant React applications using a lean + rich hybrid agent architecture.

---

## What You're Building

Design Mode is a 4-step wizard workflow that generates complete UI design systems through orchestrated specialist agents:

1. **Wizard Initialization** → User answers 10 questions (project type, aesthetic, performance targets)
2. **Agent Orchestration** → 7 specialist agents collaborate to generate components
3. **Quality Validation** → Enforce 5 Design Quality Principles + WCAG AA + performance budgets
4. **Deliverable Export** → Production-ready React components with Shadcn UI + Tailwind + TypeScript

---

## Architecture Pattern: Lean + Rich Hybrid

### 3-Step Workflow (Every Agent Follows This)

```
STEP 1: GATHER
├── Extract requirements from user request
├── Ask clarifying questions ONLY if critical info missing
└── Define success criteria

STEP 2: ACCESS KNOWLEDGE BASE
├── Read required KB files from /knowledge-bases/design-kb/
├── Extract relevant frameworks (5 Design Quality Principles, etc.)
└── Reference design system standards

STEP 3: EXECUTE
├── Apply extracted knowledge to task
├── Generate deliverables (code, specs, recommendations)
├── Self-validate against 5 Design Quality Principles
└── Return structured output to orchestrator
```

### Knowledge Base Structure

All agents reference these external knowledge bases (instead of embedding):

```
/knowledge-bases/design-kb/
├── 5-design-quality-principles.md (MANDATORY - read first)
├── component-patterns.md
├── react-best-practices.md
├── typescript-patterns.md
├── aesthetic-styles.md
├── color-systems.md
├── typography-systems.md
├── aria-patterns.md
├── keyboard-navigation.md
├── screen-reader-optimization.md
├── bundle-optimization.md
└── image-optimization.md
```

### Rich Frameworks (Embedded in Agents)

Some agents are "rich" - they embed expert frameworks for immediate capability:

- **UX Researcher** (15.3KB): Nielsen's 10 Usability Heuristics, WCAG 2.1 AA checklists, cognitive walkthrough methods
- **Animation Designer** (12.5KB): Framer Motion patterns, timing curves, GPU-acceleration techniques
- **Layout Architect** (9.2KB): Golden ratio calculations, grid system patterns, responsive breakpoints

**Why rich?** Self-contained expert knowledge = no KB lookup delays = higher quality output.

---

## File Structure Reference

```
/home/reaver47/Documents/agent-girl/designer-mode/design-mode/
│
├── BUILD_DESIGN_MODE.md           # This file - implementation guide
├── README.md                       # Complete architectural overview
├── orchestrator.md                 # Master router (ENTRY POINT)
│
├── /agents/                        # 7 Specialist Agents
│   ├── component-builder.md       # Shadcn UI + React + TypeScript
│   ├── ui-designer.md              # Design tokens, color, typography
│   ├── accessibility-specialist.md # WCAG 2.2 AA compliance validator
│   ├── performance-optimizer.md    # Bundle optimization, Core Web Vitals
│   ├── layout-architect.md         # Responsive grid systems, layouts
│   ├── animation-designer.md       # Framer Motion animations
│   └── ux-researcher.md            # Requirements validation, UX research
│
├── /master-frameworks/             # Wizard Flow Specification
│   └── wizard-flow.md              # 4-step wizard with 10 questions
│
├── /quality-checklists/            # Validation Requirements
│   └── design-quality-checklist.md # Mandatory quality checks
│
├── /templates/                     # Template Configurations
│   └── saas-product-launch.json    # Example: SaaS product template
│
└── /knowledge-bases/design-kb/     # External Knowledge Bases
    └── [12 KB files listed above]
```

---

## Implementation Steps

### Phase 1: Read All Specification Files

**Start with these files (in order):**

1. `README.md` - Complete architectural overview
2. `orchestrator.md` - Master router logic and workflow
3. `/master-frameworks/wizard-flow.md` - Wizard specification (10 questions)
4. `/quality-checklists/design-quality-checklist.md` - Validation requirements
5. `/knowledge-bases/design-kb/5-design-quality-principles.md` - MANDATORY quality framework

**Then read all 7 agent specifications:**

6. `component-builder.md` (6.1KB)
7. `ui-designer.md` (8.9KB)
8. `accessibility-specialist.md` (8.6KB)
9. `performance-optimizer.md` (8.2KB)
10. `layout-architect.md` (9.2KB)
11. `animation-designer.md` (12.5KB)
12. `ux-researcher.md` (15.3KB)

### Phase 2: Understand 5 Design Quality Principles

**Every agent MUST enforce these principles:**

1. **COMPOSE, DON'T COPY-PASTE**
   - Use Shadcn UI (composable primitives, lighter than full libraries)
   - Each component must be customizable, not rigid templates
   - Build from small, reusable parts (atomic design)

2. **ACCESSIBILITY FIRST (Not Last)**
   - WCAG 2.2 AA compliance MANDATORY (legal requirement)
   - Semantic HTML, ARIA labels, keyboard navigation
   - Test with screen readers, respect prefers-reduced-motion

3. **PERFORMANCE BUDGET ENFORCED**
   - MVP: < 90KB gzipped (HTML + CSS + JS)
   - Phase 2: < 130KB gzipped (with animations)
   - Core Web Vitals: LCP < 2.0s, FID < 50ms, CLS < 0.05
   - Code splitting, tree shaking, image optimization

4. **MOBILE-FIRST RESPONSIVE**
   - Design for 375px first, scale up to 1440px
   - Touch targets minimum 44×44px
   - Works on 3G networks (Time to Interactive < 3.5s)

5. **SEMANTIC HTML (Better SEO + Lighter DOM)**
   - Use `<header>`, `<nav>`, `<main>`, `<aside>`, `<footer>`
   - Proper heading hierarchy (h1 → h6)
   - Meaningful alt text, descriptive link text

**If ANY principle is violated → Agent loops back and fixes before returning.**

### Phase 3: Implement Orchestrator Logic

**Entry Point:** `orchestrator.md` - Master router that:

1. **Receives user request** → Launches wizard flow
2. **Wizard collects requirements** → 10 questions about project, aesthetic, performance targets
3. **Routes to specialist agents** → Based on wizard answers, spawns parallel agents
4. **Aggregates deliverables** → Combines agent outputs into cohesive design system
5. **Final quality audit** → Validates against 5 Design Quality Principles + WCAG AA + performance budgets
6. **Exports production code** → React components + Tailwind config + TypeScript types

**Key orchestrator capabilities:**
- Spawn 5+ agents in parallel (efficient delegation)
- Cross-reference findings between agents
- Enforce quality loop (if validation fails → agent retries)
- Generate structured deliverables (JSON + Markdown + Code)

### Phase 4: Wire Up Agent Collaboration

**Agents hand-off to each other via orchestrator:**

```
Wizard → Orchestrator → [UX Researcher + UI Designer] (parallel)
                      ↓
              [Component Builder + Layout Architect] (parallel)
                      ↓
              [Accessibility Specialist + Performance Optimizer] (parallel)
                      ↓
              [Animation Designer] (final polish)
                      ↓
              Final Quality Audit → Export Deliverables
```

**Collaboration Protocol (from agent specs):**

- **UX Researcher → UI Designer:** "Research shows users prefer card layout. Apply visual hierarchy principles: [details]"
- **UI Designer → Component Builder:** "Design system uses Inter Variable font, color tokens: primary-500, etc. Implement components with these tokens."
- **Component Builder → Accessibility Specialist:** "Components generated. Validate WCAG AA compliance, test keyboard navigation."
- **Layout Architect → Animation Designer:** "Layout has 3 scroll-triggered sections. Coordinate entry animations with 100ms stagger."
- **All Agents → Performance Optimizer:** "Bundle size check. Components total: [size]. Optimize to meet < 90KB target."

### Phase 5: Implement Wizard Flow

**Reference:** `/master-frameworks/wizard-flow.md`

**10 Questions (60-90 seconds to complete):**

1. Project type (SaaS, E-commerce, Portfolio, Marketing site, Dashboard, Blog)
2. Primary users (End-users, Stakeholders, Developers)
3. Brand aesthetic (Modern minimalist, Bold colorful, Professional corporate, etc.)
4. Animation preference (Subtle, Moderate, Playful, None)
5. Performance priority (Speed-first, Balanced, Feature-rich)
6. Accessibility level (WCAG A, AA, AAA)
7. Component complexity (Basic, Intermediate, Advanced)
8. Color mode (Light, Dark, System, Toggle)
9. Primary action focus (Lead generation, Content consumption, E-commerce, etc.)
10. Mobile vs. Desktop priority (Mobile-first, Desktop-first, Equal)

**Wizard output:** JSON configuration passed to orchestrator for agent routing.

### Phase 6: Knowledge Base Integration

**Every agent MUST read KB files before executing:**

**Example from component-builder.md:**
```markdown
### STEP 2: ACCESS KNOWLEDGE BASE

**You MUST read these KB files before building:**

1. **Component Patterns** (ALWAYS read first)
   - File: `/knowledge-bases/design-kb/component-patterns.md`
   - Extract: Shadcn UI setup, composition patterns, prop interfaces

2. **React Best Practices**
   - File: `/knowledge-bases/design-kb/react-best-practices.md`
   - Extract: Hooks usage, performance optimization, error boundaries

3. **TypeScript Patterns**
   - File: `/knowledge-bases/design-kb/typescript-patterns.md`
   - Extract: Type definitions, generics, discriminated unions
```

**Agent reads KB files → Extracts relevant knowledge → Applies to task → Returns output.**

### Phase 7: Quality Validation Loop

**Before returning ANY deliverable, agent validates:**

```markdown
## Quality Self-Evaluation Checklist

- [ ] All 5 Design Quality Principles enforced
- [ ] WCAG 2.2 AA compliance verified (aXe, WAVE)
- [ ] Performance budget met (< 90KB MVP or < 130KB Phase 2)
- [ ] Mobile-first responsive (375px → 1440px)
- [ ] Semantic HTML used (header, nav, main, aside, footer)
- [ ] Code splitting applied (lazy load below-fold)
- [ ] Tree shaking verified (named imports only)
- [ ] Images optimized (WebP/AVIF via next/image)
- [ ] Accessibility tested (keyboard nav, screen reader)
- [ ] Bundle analyzed (verified actual size)

**If any item fails → Fix before returning to orchestrator**
```

### Phase 8: Export Deliverables

**Final output structure:**

```
/design-system-export/
├── /components/               # React components (Shadcn UI + Tailwind)
│   ├── Button.tsx
│   ├── Card.tsx
│   ├── Input.tsx
│   └── [all components]
│
├── /config/                   # Configuration files
│   ├── tailwind.config.js     # Tailwind design tokens
│   ├── theme.ts               # Theme configuration (colors, typography)
│   └── framer-motion.ts       # Animation variants
│
├── /docs/                     # Documentation
│   ├── DESIGN_SYSTEM.md       # Design system overview
│   ├── ACCESSIBILITY.md       # WCAG compliance report
│   ├── PERFORMANCE.md         # Performance optimization summary
│   └── COMPONENTS.md          # Component usage guide
│
├── /assets/                   # Optimized assets
│   ├── /images/               # WebP/AVIF images
│   └── /fonts/                # Variable fonts (subset)
│
└── package.json               # Dependencies (React, Shadcn, Framer Motion, etc.)
```

---

## Integration Requirements

### Mode Activation

Design Mode should be activated by user typing:

```
/design
```

**Or explicitly requesting:**
```
"Enter design mode"
"Generate a design system"
"Build UI components for [project]"
```

### Mode Isolation

**CRITICAL:** Design Mode must NOT contaminate other modes:

- Only activate when explicitly requested
- Use separate context/state from other modes
- Don't leak design-specific language to other modes
- Exit cleanly when user switches modes

### User Experience Flow

```
User: "/design"
      ↓
Design Mode Wizard Launches
      ↓
10 questions (60-90 seconds)
      ↓
Orchestrator spawns agents (parallel execution)
      ↓
Progress updates: "UX Researcher analyzing... UI Designer generating tokens..."
      ↓
Quality validation (5 principles + WCAG AA + performance)
      ↓
Export deliverables (React components + docs + config)
      ↓
User receives: "✅ Design system complete! 12 components generated. Bundle: 87KB. WCAG AA compliant."
```

---

## Performance Targets

**Design Mode must be FAST:**

- Wizard completion: < 90 seconds
- Agent execution (7 agents parallel): < 3 minutes
- Total time from start to deliverables: < 5 minutes

**Bundle targets for GENERATED code:**

- MVP: < 90KB gzipped (HTML + CSS + JS)
- Phase 2: < 130KB gzipped (with animations)
- Core Web Vitals: LCP < 2.0s, FID < 50ms, CLS < 0.05

---

## Testing Checklist (For Minimal Claude)

Before marking Design Mode as complete, verify:

- [ ] **Wizard Flow:** All 10 questions render correctly, answers captured
- [ ] **Orchestrator:** Routes to correct agents based on wizard answers
- [ ] **Agent Execution:** All 7 agents execute without errors
- [ ] **KB File Access:** Agents successfully read knowledge base files
- [ ] **5 Principles Enforcement:** All agents validate against quality framework
- [ ] **WCAG AA Compliance:** Generated components pass accessibility audit
- [ ] **Performance Budget:** Generated code meets < 90KB target
- [ ] **Deliverables Export:** All files generated (components, config, docs)
- [ ] **Mode Isolation:** Design Mode doesn't contaminate other modes
- [ ] **Error Handling:** Graceful failures, helpful error messages

---

## Edge Cases & Error Handling

### Case 1: User Skips Wizard Questions

**Behavior:** Use template defaults (e.g., saas-product-launch.json)

**User message:** "Using SaaS Product Launch template. Type /design-wizard to customize."

### Case 2: Bundle Size Exceeds Target

**Behavior:** Performance Optimizer agent identifies heaviest components, suggests removals

**User message:** "⚠️ Bundle: 112KB (target: 90KB). Remove [Feature X] or defer [Component Y] to meet budget?"

### Case 3: WCAG AA Validation Fails

**Behavior:** Accessibility Specialist loops back, fixes issues, re-validates

**User message:** "❌ Accessibility issue: Missing ARIA labels. Fixing... ✅ WCAG AA compliant."

### Case 4: Knowledge Base File Missing

**Behavior:** Agent uses embedded fallback knowledge, logs warning

**User message:** "⚠️ KB file not found: component-patterns.md. Using fallback patterns. Quality may be reduced."

---

## Quality Standards

**Design Mode output must be:**

1. **Production-ready** - No placeholders, no TODOs, fully functional code
2. **Accessible** - WCAG 2.2 AA compliant (legal requirement)
3. **Performant** - Meets Core Web Vitals targets (LCP < 2.0s, FID < 50ms, CLS < 0.05)
4. **Responsive** - Works on 375px mobile to 1440px desktop
5. **Documented** - Clear usage guides, component props documented
6. **Type-safe** - Full TypeScript coverage, no `any` types
7. **Tested** - Validation against 5 Design Quality Principles + performance budgets

---

## Final Notes

**Architecture Pattern:** Lean + Rich Hybrid
- Agents reference KB files (lean)
- Core expert knowledge embedded in agents (rich)
- 3-Step Workflow enforced (Gather → Access KB → Execute)

**Total File Size:** 69KB (18% reduction from original 84KB)

**Quality:** ⬆️ HIGHER (embedded expert frameworks make agents immediately capable)

**Mirrors Supah Copy Mazta Pattern:**
- Orchestrator routes to specialist agents
- Embedded frameworks (Nielsen's heuristics = Russell Brunson frameworks)
- Quality enforcement via self-validation
- Rich versions preferred for expert capability

**GitHub Repository:** https://github.com/creativeprofit22/design-mode

**Local Path:** `/home/reaver47/Documents/agent-girl/designer-mode/design-mode/`

---

## Success Criteria

Design Mode is complete when:

1. ✅ User can type `/design` and wizard launches
2. ✅ Wizard collects 10 answers in < 90 seconds
3. ✅ Orchestrator spawns 7 agents in parallel
4. ✅ Agents read KB files and apply knowledge
5. ✅ All deliverables validate against 5 Design Quality Principles
6. ✅ Generated components are WCAG AA compliant
7. ✅ Bundle size meets < 90KB target (MVP) or < 130KB (Phase 2)
8. ✅ Exports production-ready React components + docs + config
9. ✅ Mode isolates cleanly (no contamination of other modes)
10. ✅ Total execution time < 5 minutes (wizard → deliverables)

---

**Good luck, Minimal Claude! Build something brilliant. 🚀**

**Remember:** Quality over speed. If validation fails, loop back and fix. Users expect WCAG AA compliance and performance budgets are non-negotiable. You are the guardian of production-ready design systems.

**Any questions?** Reference `/home/reaver47/Documents/agent-girl/designer-mode/design-mode/README.md` for complete architectural overview.
