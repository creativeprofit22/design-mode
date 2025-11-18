# Build Design Mode - Implementation Prompt

**Location:** `/home/reaver47/Documents/agent-girl/chat-9f130bd1/design-mode/`

---

## Task: Implement Design Mode for Agent Girl

Build a wizard-based design system generator that creates production-ready, accessible, performant React applications.

### Reference Documentation

**Start here:** `README.md` - Complete architectural overview

**Core specs:**
- `orchestrator.md` - Master router with 4-step wizard flow
- `/agents/` - 7 specialist agent specifications
- `/master-frameworks/wizard-flow.md` - Complete wizard specification
- `/quality-checklists/design-quality-checklist.md` - Validation requirements
- `/templates/saas-product-launch.json` - Template configuration example

---

## ✅ REFACTORING COMPLETE (Nov 18, 2025)

**Status:** Lean Architecture Implementation Complete

### What Was Done

All 7 specialist agents were refactored from bloated (embedded examples) to **lean + rich hybrid architecture**:

**Architecture Pattern:**
- **3-Step Workflow:** Gather → Access KB → Execute (consistent across all agents)
- **KB File References:** Agents reference external knowledge bases instead of embedding all content
- **Rich Frameworks Embedded:** Core expert knowledge kept in agents (Nielsen's heuristics, Framer Motion patterns, WCAG checklists)
- **5 Design Quality Principles:** Mandatory enforcement across all agents
- **Anti-Patterns:** What NOT to do (prevents common mistakes)
- **Collaboration Protocol:** How agents hand-off to each other

**Results:**
- **Before:** 84KB (bloated with embedded examples)
- **After:** 69KB (lean + rich hybrid)
- **Reduction:** 18% (15KB saved)
- **Quality:** ⬆️ HIGHER (embedded expert frameworks make agents immediately capable)

**Files Created:**
- ✅ 11 Knowledge Base files (component-patterns, react-best-practices, etc.)
- ✅ 5 Design Quality Principles framework
- ✅ 7 Refactored agents (component-builder, ui-designer, accessibility, performance, layout, animation, ux-researcher)

**GitHub Repository:**
- ✅ Repository created: https://github.com/creativeprofit22/design-mode
- ✅ Initial commit pushed with clean version history
- ✅ All refactored files committed (28 files, 11,470 lines)
- ✅ Public repository with full documentation

**Trade-off Decision:**
Kept **rich versions** (12-15KB each) instead of **lean versions** (6KB each) because:
- Self-contained expert knowledge (Nielsen's 10 heuristics, Framer Motion patterns, WCAG checklists)
- Immediately actionable (no KB lookup delays)
- Mirrors Supah Copy Mazta pattern (embedded frameworks: Russell Brunson, Tony Robbins)
- Higher quality agents worth the extra 6-9KB per file

**Next Steps:**
1. Testing with Minimal Claude (after mode is built)
2. Integration into Agent Girl (after testing)

---

## THE PATTERN (Supah Copy Mazta Architecture)

Design Mode follows the **exact same architecture** as Supah Copy Mazta mode. Here's what that means specifically:

### 1. Orchestrator Pattern

**File reference:** `/home/reaver47/Documents/agent-girl/design-mode-plan/supah-orchestrator.md`

**What it does:**
- **Master Router:** Analyzes incoming requests, routes to correct specialist agent
- **NOT a worker:** Orchestrator NEVER does the work itself - it delegates
- **Quality Enforcer:** Loops back if specialist returns poor quality work
- **Mode Isolation:** Scans outputs for forbidden references, blocks contamination

**Routing Logic Example (from Supah Copy Mazta):**
```
IF request contains "VSL", "webinar script", "sales script"
  → Route to: sales-script-writer

IF request contains "discovery call", "closing call"
  → Route to: call-script-writer

IF request contains "Facebook ads", "campaign", "funnel"
  → Route to: campaign-orchestration

IF request is NEW project:
  → Start wizard flow

IF request is vague:
  → Ask clarifying questions FIRST, then route
```

**Design Mode equivalent:**
```
IF request contains "component", "button", "card"
  → Route to: component-builder

IF request contains "layout", "grid", "dashboard"
  → Route to: layout-architect

IF request contains "accessibility", "WCAG", "a11y"
  → Route to: accessibility-specialist

IF request is NEW design project:
  → Start 4-step wizard flow

IF request is vague:
  → Ask clarifying questions FIRST, then route
```

**Key principle:** Orchestrator = intelligent router, NOT worker. It coordinates, never executes.

---

### 2. 4-Step Wizard Flow

**File reference:** `/home/reaver47/Documents/agent-girl/design-mode-plan/SUPAH_COPY_MAZTA_HANDOFF.md` (lines 72-121)

**The exact pattern:**

```
Step 1: Template Select
├─ Present 6 options with icons + descriptions
├─ User picks one
└─ Load template-specific configuration

Step 2: Brief Input
├─ Template-specific questions (dynamic based on Step 1 selection)
├─ Gather all required information
├─ AUTO-RESEARCH TRIGGER: If user says "I don't know"
└─ Specialist validates completeness (background)

Step 3: Voice & Tone (for Copy) / Style & Preferences (for Design)
├─ Present 3-4 options with examples
├─ AUTO-RESEARCH TRIGGER: If user asks "Which should I choose?"
├─ User selects
└─ Specialist generates preview (background)

Step 4: Review
├─ Show complete summary of all selections
├─ Run quality validation (background)
├─ User confirms OR goes back to change
└─ Execute: Route to specialists in sequence

Then: Orchestrator coordinates all specialists → Returns complete deliverable
```

**Critical details:**

- **Progressive disclosure:** Only show one step at a time, don't overwhelm
- **State management:** Wizard maintains context across all 4 steps
- **No skipping:** User MUST complete all steps for new projects
- **Auto-save:** Progress saved at each step (user can pause/resume)
- **Validation gate:** Step 4 runs quality checks BEFORE user confirms

**Supah Copy Mazta example (Step 1):**
```
SELECT A TEMPLATE:

📹 VSL Script (5-20 min video sales letter)
🎤 Webinar Script (45-90 min presentation)
📞 Discovery Call (30-60 min qualification call)
📲 Closing Call (20-40 min enrollment call)
🎯 Facebook Ads Funnel (Complete campaign)
📄 Landing Page (Sales letter foundation)
```

**Design Mode equivalent (Step 1):**
```
SELECT A TEMPLATE:

📄 Landing Pages
  → SaaS Product Launch (Hero, Features, Pricing, CTA)
  → Lead Generation (Hero, Benefits, Form)
  → App Download (Hero, App Preview, Download CTAs)

🎛️ Dashboards & Apps
  → Admin Dashboard (Sidebar, Stats, Tables, Charts)
  → Analytics Dashboard (Metrics, Graphs, Filters)

🧩 Component Libraries
  → Design System from Scratch
```

---

### 3. Auto-Research Mode

**File reference:** `/home/reaver47/Documents/agent-girl/design-mode-plan/supah-orchestrator.md` (lines 108-129)

**When it triggers:**

```
USER: "Write me a sales script"
ORCHESTRATOR: "I need more context. Let me ask:
1. What's the offer?
2. Who's the target audience?
3. What format?

If you don't have this information yet, I can help you research and develop it."
```

**Research Process (exact steps):**

1. **Identify missing information**
   - User says "I don't know"
   - User asks "What's best?" or "What should I use?"
   - Agent detects incomplete brief

2. **Use WebSearch tool** to research
   - Search: "[topic] best practices 2025"
   - Search: "[topic] conversion optimization"
   - Search: "[topic] examples"

3. **Present findings with sources**
   ```
   "Based on research, here are the top options:

   1. Option A (Recommended)
      - Best for: [use case]
      - Pros: [specific benefits]
      - Used by: [real examples]
      - Source: [link]

   2. Option B
      - Best for: [use case]
      - Pros: [specific benefits]

   RECOMMENDATION: [Option A] because [specific rationale]

   Proceed with this?"
   ```

4. **User validates/refines**
   - User accepts → Continue with that choice
   - User rejects → Research alternative
   - User provides own info → Use that instead

5. **Continue wizard flow** with validated information

**Design Mode example:**

```
USER: "I'm not sure what design system to use"

ORCHESTRATOR:
[Triggers WebSearch]
"Let me research the best design systems for 2025...

[WebSearch: "design systems 2025 performance comparison"]
[WebSearch: "Shadcn UI vs Material UI vs Chakra"]

Based on research:

1. Shadcn UI + Tailwind (RECOMMENDED)
   - Bundle: ~32KB base (vs 120KB Material UI)
   - Accessibility: WCAG AA built-in (Radix primitives)
   - Used by: Vercel, Linear, Supabase
   - Source: Shadcn UI docs, GitHub analytics

2. Park UI
   - Bundle: ~78KB
   - Pros: Pre-built patterns, faster setup
   - Cons: Less flexible than Shadcn

3. Material UI
   - Bundle: ~120KB minimum
   - Cons: Heavy, dated aesthetic, hard to customize

For your dashboard, I recommend Shadcn UI because:
- 75% lighter than Material UI
- Modern aesthetic (2025 standard)
- Full control (copy-paste, not npm package)

Proceed with Shadcn UI?"

[User responds]
```

**Key principle:** Don't assume or guess. If user lacks knowledge, RESEARCH it, present options, let user decide with full information.

---

### 4. Specialist Agent Coordination

**File reference:** `/home/reaver47/Documents/agent-girl/design-mode-plan/supah-orchestrator.md` (lines 133-175)

**How it works:**

**Example: Full Campaign Build (Supah Copy Mazta)**
```
REQUEST: "I need a complete campaign for my coaching offer"

ORCHESTRATOR WORKFLOW:
1. Gather intel (offer, audience, budget, unique mechanism)
2. Route to Campaign Orchestration → Strategy + ad copy
3. Campaign Orchestration coordinates with Landing Page Writer → Sales letter
4. Route to Call Script Writer → Discovery + closing scripts
5. Synthesize outputs → Present complete package

DELIVERABLES:
✓ Campaign strategy
✓ Ad copy library
✓ Landing page (5,000+ word sales letter)
✓ Email sequences
✓ Call scripts
✓ KPI dashboard
```

**Design Mode equivalent:**
```
REQUEST: "I need a SaaS product launch landing page"

ORCHESTRATOR WORKFLOW:
1. Execute wizard (gather template, content, style preferences)
2. Route to UX Researcher → Validate requirements
3. Route to UI Designer → Generate design tokens + mockup options
4. Route to Component Builder → Generate React components
5. Route to Layout Architect → Integrate components into page structure
6. Route to Animation Designer → Add micro-interactions
7. Route to Accessibility Specialist → Final WCAG audit
8. Route to Performance Optimizer → Bundle optimization
9. Synthesize outputs → Present complete app

DELIVERABLES:
✓ Complete Next.js 15 app structure
✓ All React components (Hero, Features, Pricing, etc.)
✓ Tailwind config with design tokens
✓ Responsive layout (mobile, tablet, desktop)
✓ Dark mode support
✓ WCAG AA accessible
✓ Performance optimized (< 90KB bundle)
✓ README with setup instructions
```

**Coordination Rules (from Supah Copy Mazta):**

1. **Gather complete intel FIRST**
   - Don't route with incomplete info
   - Use wizard to collect everything upfront
   - Validate completeness before proceeding

2. **Route in logical sequence**
   - Strategy before content
   - Design before build
   - Build before optimize
   - Validate last (QA gate)

3. **Ensure consistency across outputs**
   - Same design tokens in all components
   - Same messaging in all copy
   - No contradictions between specialists

4. **Present complete package**
   - Don't return partial outputs
   - Synthesize everything together
   - One coherent deliverable, not scattered files

**Key principle:** Orchestrator is the conductor. Each specialist plays their instrument. Orchestrator ensures harmony.

---

### 5. Mode Isolation Enforcement

**File reference:** `/home/reaver47/Documents/agent-girl/design-mode-plan/supah-orchestrator.md` (lines 82-105)

**Supah Copy Mazta isolation rules:**

**Forbidden references:**
- Gary Halbert
- Dan Kennedy
- Chief Neefe
- Sabri Suby
- Any legacy copywriting frameworks

**Permitted frameworks ONLY:**
1. Russell Brunson (Value Ladder, Offers, Funnels)
2. Tony Robbins (State Management, Sales Call)
3. Elite Copywriting Magnetism
4. High-Ticket Sales
5. Facebook Ads Mastery
6. Unique Mechanism & IP

**Enforcement:**
- Scan all outputs for forbidden references
- If detected → Immediately rewrite without those references
- Use ONLY the 6 permitted frameworks

**Design Mode equivalent:**

**Forbidden references:**
- Copywriting frameworks (Gary Halbert, Russell Brunson, Tony Robbins, etc.)
- Marketing copy swipe files
- Supah Copy Mazta knowledge bases
- Any copy-specific terminology (unless designing FOR copy)

**Permitted frameworks ONLY:**
1. Shadcn UI + Radix UI (component architecture)
2. Tailwind CSS v4 (styling system)
3. Framer Motion (animation patterns)
4. WCAG 2.2 (accessibility standards)
5. Next.js 15 + React 19 (framework patterns)
6. Performance Budgets (Core Web Vitals, bundle optimization)

**Enforcement:**
- Scan all outputs for forbidden references
- If detected → Clarify scope (design vs copy)
- Design Mode generates DESIGN (layouts, components, styles)
- NOT copy content (headlines, taglines, CTAs)

**Exception:** Phase 2 - MCP protocol can REQUEST copy from Supah Copy Mazta
- Design Mode: "I need copy for this hero section"
- MCP → Supah Copy Mazta: [isolated request]
- Supah Copy Mazta: [returns copy]
- Design Mode: [integrates copy into component]
- No contamination (Design Mode doesn't learn copywriting, just receives content)

**Key principle:** Modes are COMPLETELY isolated. Design stays in design lane. Copy stays in copy lane. They can collaborate (via MCP) but never contaminate.

---

### 6. Quality Enforcement Workflow

**File reference:** `/home/reaver47/Documents/agent-girl/design-mode-plan/supah-orchestrator.md` (lines 44-78)

**Supah Copy Mazta quality flow:**

```
1. Route to specialist agent (NEVER write copy yourself)

2. Specialist agent MUST:
   - Read quality checklist BEFORE writing
   - Calculate quality thresholds
   - Return output + quality audit table
   - Self-assess: PASS / REWRITE NEEDED
   - Only return PASS-quality work

3. Orchestrator verifies audit included
   - If no audit → Send back with requirement

4. Pass to quality auditor for final check

5. Quality auditor returns verdict:
   - PASS ✅ → Deliver to user
   - FAIL ❌ → Loop back to specialist with specific fixes

6. If FAIL: Send specialist back with:
   - Specific elements that failed
   - Exact fixes needed
   - Require rewrite and re-audit

NO EXCEPTIONS: Quality standards mandatory
```

**Design Mode equivalent:**

```
1. Route to specialist agent (NEVER generate design yourself)

2. Specialist agent MUST:
   - Read quality checklist BEFORE generating
   - Follow design standards (WCAG AA, bundle budgets)
   - Return output + metadata (bundle size, accessibility notes)
   - Self-validate against checklist
   - Only return passing work

3. Orchestrator verifies metadata included
   - If no metadata → Send back with requirement

4. Pass to Accessibility Specialist for WCAG audit
5. Pass to Performance Optimizer for bundle check

6. Quality gates return verdict:
   - Accessibility: PASS ✅ / FAIL ❌
   - Performance: PASS ✅ / FAIL ❌

7. If ANY FAIL: Loop back to component with specific fixes:
   - "Primary button contrast 4.2:1, needs ≥ 4.5:1 → Darken to #2563eb"
   - "Bundle 124KB, target < 90KB → Lazy load Pricing, Testimonials"

NO EXCEPTIONS: Accessibility + Performance mandatory
```

**Quality standards (from Supah Copy Mazta):**
- "150-200% above baseline on conversational elements"
- All copy must hit specific quality thresholds
- No delivery until passing

**Design Mode standards:**
- WCAG 2.2 Level AA (no exceptions)
- Bundle < 90KB gzipped (MVP)
- LCP < 2.0s, FID < 50ms, CLS < 0.05
- No delivery until passing

**Key principle:** Quality is enforced, not optional. Specialist agents self-validate. Quality specialists do final audit. Loop back if fails. User only sees passing work.

---

## What to Build (MVP - Phase 1)

### 1. Orchestrator Agent
- File: `orchestrator.md` (already created - read it)
- **Function:** Routes requests, executes 4-step wizard, coordinates specialist agents
- **Pattern:** Exactly like Supah Copy Mazta orchestrator (see above)
- **Key features:**
  - Auto-research triggers (WebSearch when user says "I don't know")
  - Mode isolation enforcement (no copywriting references)
  - Quality enforcement (loop back if fails)

### 2. Specialist Agents (3 for MVP, 7 total)

**MVP (Phase 1):**
- `component-builder.md` - Generate React/TypeScript components
- `ui-designer.md` - Design tokens, Tailwind config
- `accessibility-specialist.md` - WCAG 2.2 AA validation

**Each agent MUST:**
- Read quality checklist BEFORE generating
- Return output + metadata
- Self-validate against standards
- Only return passing work

**Phase 2:**
- `performance-optimizer.md` - Bundle optimization
- `ux-researcher.md` - Requirements validation
- `layout-architect.md` - Grid systems, layouts
- `animation-designer.md` - Framer Motion animations

### 3. 4-Step Wizard Flow

**Implementation checklist:**

**Step 1: Template Selection**
- [ ] Present 3 templates (SaaS, Lead Gen, App Download)
- [ ] Clear descriptions + icons (like Supah Copy Mazta)
- [ ] "Not Sure?" option → Auto-research mode
- [ ] Load template config when selected
- [ ] Save state

**Step 2: Requirements Gathering**
- [ ] Load template-specific questions (from JSON config)
- [ ] Dynamic form based on template
- [ ] "I need help brainstorming" button → Auto-research
- [ ] Validate completeness
- [ ] Background: UX Researcher validates
- [ ] Save state

**Step 3: Style & Preferences**
- [ ] Present 4 aesthetics (Minimal, Bold, Glassmorphism, Playful)
- [ ] "Recommend based on my brief" → Auto-research
- [ ] Color palette selection
- [ ] Typography selection
- [ ] Special features checkboxes
- [ ] Background: UI Designer generates mockup options
- [ ] Save state

**Step 4: Review & Confirmation**
- [ ] Show complete summary (all steps)
- [ ] Background: Accessibility check (color contrast)
- [ ] Background: Performance estimate (bundle size)
- [ ] Display validation results
- [ ] "Change something" → Go back to specific step
- [ ] "Generate" → Execute coordination
- [ ] Save final state

**After confirmation:**
- [ ] Orchestrator coordinates specialists in sequence
- [ ] Progress indicator shown to user
- [ ] Return complete deliverable
- [ ] No partial outputs

### 4. Auto-Research Implementation

**Trigger detection:**
```typescript
// Pseudo-code
if (userInput.includes("I don't know")) {
  triggerAutoResearch(currentContext)
}

if (userInput.includes("What should I")) {
  triggerAutoResearch(currentContext)
}

if (userAsksQuestion && lackingInformation) {
  triggerAutoResearch(currentContext)
}
```

**Research execution:**
```typescript
async function autoResearch(topic: string, context: object) {
  // 1. Formulate search queries
  const queries = [
    `${topic} best practices 2025`,
    `${topic} examples`,
    `${topic} comparison`,
  ]

  // 2. Execute WebSearch
  const results = await Promise.all(
    queries.map(q => webSearch(q))
  )

  // 3. Synthesize findings
  const findings = synthesizeResults(results)

  // 4. Present to user with recommendation
  return {
    options: findings.options,
    recommendation: findings.best,
    rationale: findings.reasoning,
    sources: findings.sources,
  }
}
```

**Presentation format:**
```
"Based on research, here are the top options:

1. [Option A] (RECOMMENDED)
   - Best for: [specific use case]
   - Pros: [specific benefits]
   - Used by: [real examples]
   - Source: [citation]

2. [Option B]
   - Best for: [specific use case]
   - Pros: [specific benefits]

RECOMMENDATION: [Option A] because [specific rationale based on user's brief]

Proceed with this?"
```

### 5. Output Generation

**Final deliverable structure:**
```
generated-design/
├── app/
│   ├── layout.tsx (root layout with metadata)
│   └── page.tsx (landing page)
├── components/
│   ├── ui/ (Shadcn components)
│   │   ├── button.tsx
│   │   ├── card.tsx
│   │   └── ...
│   ├── Hero.tsx
│   ├── Features.tsx
│   ├── Pricing.tsx
│   └── ...
├── lib/
│   ├── utils.ts (cn helper)
│   └── animations.ts (Framer Motion variants)
├── public/
│   └── (user-provided images)
├── tailwind.config.ts (design tokens)
├── package.json
├── tsconfig.json
└── README.md (setup instructions)
```

**Quality validation before delivery:**
- ✅ Accessibility Specialist audit passed
- ✅ Performance Optimizer check passed
- ✅ Bundle < 90KB gzipped
- ✅ All TypeScript errors resolved
- ✅ Build succeeds
- ✅ README included

---

## Tech Stack (Recommended)

**Design System:** Shadcn UI + Radix UI
- Copy-paste components (NOT npm package)
- ~32KB base, WCAG AA built-in
- See: `knowledge-bases/design-systems-2025.md`

**Styling:** Tailwind CSS v4
- Utility-first, 50% smaller CSS
- No runtime cost

**Framework:** Next.js 15 + React 19
- Server components
- Edge deployment ready

**Animations:** Framer Motion (Phase 2)
- 28KB, GPU-accelerated
- See: `agents/animation-designer.md`

---

## Quality Standards (Non-Negotiable)

**Accessibility:**
- ✅ WCAG 2.2 Level AA compliant
- ✅ Color contrast ≥ 4.5:1 (text), ≥ 3:1 (UI)
- ✅ Keyboard navigation, screen reader optimized
- **Gatekeeper:** Accessibility Specialist agent validates before delivery

**Performance:**
- ✅ Bundle < 90KB gzipped (MVP)
- ✅ LCP < 2.0s, FID < 50ms, CLS < 0.05
- ✅ Code splitting, tree shaking, image optimization
- **Gatekeeper:** Performance Optimizer agent validates before delivery

**Code Quality:**
- ✅ TypeScript (strict mode, no `any`)
- ✅ Functional components with hooks
- ✅ Production-ready (no cleanup needed)

**Quality enforcement:**
- Specialists self-validate
- Quality gates run final checks
- Loop back if fails (with specific fixes)
- User only sees passing work

---

## Implementation Steps

1. **Read the specs:**
   - `README.md` - Complete overview
   - `orchestrator.md` - Routing logic, wizard flow
   - `master-frameworks/wizard-flow.md` - Detailed wizard specification
   - **THIS FILE** - Pattern explanation (you're reading it)

2. **Study Supah Copy Mazta reference:**
   - `/home/reaver47/Documents/agent-girl/design-mode-plan/supah-orchestrator.md`
   - `/home/reaver47/Documents/agent-girl/design-mode-plan/SUPAH_COPY_MAZTA_HANDOFF.md`
   - Understand: Orchestrator routing, wizard flow, auto-research, quality enforcement

3. **Build Phase 1 (MVP):**
   - Orchestrator with 4-step wizard
   - 3 core agents (Component Builder, UI Designer, Accessibility)
   - 1 template (SaaS Product Launch)
   - Auto-research integration
   - Quality validation (accessibility, performance)

4. **Test:**
   - Walk through wizard (all 4 steps)
   - Trigger auto-research (say "I don't know")
   - Generate SaaS landing page
   - Validate output quality (WCAG AA, bundle size, code quality)

5. **Iterate:**
   - Add Phase 2 agents if needed
   - Expand templates
   - Refine based on usage

---

## Success Criteria

**MVP is complete when:**

- ✅ 4-step wizard functional (Template → Brief → Style → Review)
- ✅ Auto-research triggers work correctly:
  - User says "I don't know" → WebSearch executes
  - Findings presented with options + recommendation
  - User validates → Wizard continues with choice
- ✅ 3 specialist agents coordinate correctly:
  - Orchestrator routes (doesn't do work itself)
  - Specialists return output + metadata
  - Quality gates validate before delivery
- ✅ Generated output passes ALL quality checks:
  - WCAG 2.2 AA compliant (Accessibility Specialist validates)
  - Bundle < 90KB gzipped (Performance Optimizer validates)
  - LCP < 2.0s (estimated)
  - Production-ready TypeScript/React code
  - Build succeeds, no errors
- ✅ SaaS Product Launch template generates complete landing page:
  - Hero, Features, Pricing, Testimonials, CTA, Footer
  - Responsive (mobile, tablet, desktop)
  - Dark mode support
  - README with setup instructions
- ✅ Mode isolation enforced:
  - No copywriting framework references in output
  - Design Mode stays in design lane

---

## Documentation Reference

**All specifications in:** `/home/reaver47/Documents/agent-girl/chat-9f130bd1/design-mode/`

**Quick links:**
- Overview: `README.md`
- Orchestrator: `orchestrator.md`
- Wizard flow: `master-frameworks/wizard-flow.md`
- Agents: `/agents/*.md`
- Quality: `quality-checklists/design-quality-checklist.md`
- Templates: `/templates/*.json`
- Knowledge: `/knowledge-bases/*.md`

**Pattern reference (Supah Copy Mazta):**
- `/home/reaver47/Documents/agent-girl/design-mode-plan/supah-orchestrator.md`
- `/home/reaver47/Documents/agent-girl/design-mode-plan/SUPAH_COPY_MAZTA_HANDOFF.md`
- `/home/reaver47/Documents/agent-girl/design-mode-plan/SUPAH_COPY_MAZTA_ISOLATED_MODE_ARCHITECTURE.md`

---

## Key Takeaways

**The Pattern = 6 Things:**

1. **Orchestrator Routes, Never Executes** - Intelligent delegation, not worker
2. **4-Step Wizard for New Projects** - Progressive disclosure, complete requirements
3. **Auto-Research When User Lacks Knowledge** - WebSearch → Present options → User validates
4. **Specialist Agent Coordination** - Sequence matters, ensure consistency
5. **Mode Isolation Enforcement** - No contamination, scan outputs
6. **Quality Enforcement Workflow** - Self-validate, final audit, loop back if fails

**Build it exactly like Supah Copy Mazta, but for design instead of copy.**

---

**Build it lean. Build it right. Make it amazing.**
