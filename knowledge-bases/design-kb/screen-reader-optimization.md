# Screen Reader Optimization

**Purpose:** Screen reader best practices and optimization techniques

---

## Screen Reader Basics

### Major Screen Readers

| Platform | Screen Reader | Market Share |
|----------|---------------|--------------|
| Windows | NVDA (free) | ~40% |
| Windows | JAWS (commercial) | ~30% |
| macOS/iOS | VoiceOver (built-in) | ~20% |
| Android | TalkBack (built-in) | ~10% |

---

## SR-Only (Screen Reader Only) Text

### When to Use

Add context that's obvious visually but not to screen readers:

```tsx
// Icon-only button
<button>
  <X className="h-4 w-4" aria-hidden="true" />
  <span className="sr-only">Close dialog</span>
</button>

// External link indicator
<a href="https://example.com" target="_blank">
  Visit example.com
  <span className="sr-only">(opens in new window)</span>
</a>

// Status icons
<div>
  <Check className="text-green-600" aria-hidden="true" />
  <span className="sr-only">Success: </span>
  <span>Task completed</span>
</div>
```

---

### SR-Only CSS Class

```css
/* Tailwind provides sr-only, but here's how it works: */
.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border-width: 0;
}

/* Make visible on focus (for skip links) */
.sr-only:focus {
  position: static;
  width: auto;
  height: auto;
  padding: inherit;
  margin: inherit;
  overflow: visible;
  clip: auto;
  white-space: normal;
}
```

---

### Tailwind Usage

```tsx
// Hide visually, show to SR
<span className="sr-only">Additional context</span>

// Show on focus (skip links)
<a href="#main" className="sr-only focus:not-sr-only">
  Skip to content
</a>
```

---

## Image Alt Text

### Descriptive Alt Text

```tsx
// ✅ GOOD: Descriptive, informative
<Image
  src="/chart.png"
  alt="Revenue growth chart showing 40% increase from Q1 to Q2 2024"
  width={600}
  height={400}
/>

// ✅ GOOD: Context-specific
<Image
  src="/team-photo.jpg"
  alt="5 team members smiling at company retreat in Colorado"
  width={800}
  height={600}
/>

// ❌ BAD: Too generic
<Image src="/chart.png" alt="Chart" width={600} height={400} />

// ❌ BAD: Redundant "image of"
<Image src="/chart.png" alt="Image of a chart" width={600} height={400} />
```

---

### Decorative Images

```tsx
// ✅ GOOD: Empty alt for decorative
<Image
  src="/decoration.svg"
  alt=""
  aria-hidden="true"
  width={100}
  height={100}
/>

// Background patterns (CSS, no alt needed)
<div className="bg-pattern" aria-hidden="true">
  Content
</div>
```

---

### Complex Images (Charts, Diagrams)

```tsx
// Provide alt + long description
<figure>
  <Image
    src="/complex-chart.png"
    alt="Sales data for 2024 by region"
    aria-describedby="chart-description"
    width={800}
    height={600}
  />
  <figcaption id="chart-description" className="sr-only">
    Sales data showing North America: $2.5M (45%), Europe: $1.8M (32%),
    Asia: $1.2M (23%). North America grew 18% year-over-year.
  </figcaption>
</figure>

// Or provide data table alternative
<div>
  <Image src="/chart.png" alt="Regional sales data (see table below)" />
  <table className="sr-only">
    <caption>Regional Sales 2024</caption>
    <tr>
      <th>Region</th>
      <th>Sales</th>
      <th>Growth</th>
    </tr>
    {/* Data rows */}
  </table>
</div>
```

---

## Link Text

### Descriptive Links

```tsx
// ✅ GOOD: Descriptive, standalone
<a href="/pricing">View pricing plans</a>
<a href="/docs/api">Read API documentation</a>

// ❌ BAD: Generic "click here" / "read more"
<p>
  For pricing information, <a href="/pricing">click here</a>.
</p>

// ✅ FIX: Add context via aria-label
<p>
  For pricing information,{' '}
  <a href="/pricing" aria-label="View pricing information">
    click here
  </a>
  .
</p>
```

---

### Links in Lists

```tsx
// ✅ GOOD: Each link is descriptive
<ul>
  <li><a href="/features/analytics">Analytics Dashboard</a></li>
  <li><a href="/features/reports">Custom Reports</a></li>
  <li><a href="/features/alerts">Real-time Alerts</a></li>
</ul>

// ❌ BAD: All links say "Learn more"
<ul>
  <li>
    Analytics Dashboard <a href="/features/analytics">Learn more</a>
  </li>
  <li>
    Custom Reports <a href="/features/reports">Learn more</a>
  </li>
</ul>

// ✅ FIX: Add context
<ul>
  <li>
    Analytics Dashboard{' '}
    <a href="/features/analytics" aria-label="Learn more about Analytics Dashboard">
      Learn more
    </a>
  </li>
</ul>
```

---

## Form Labels

### Explicit Labels

```tsx
// ✅ GOOD: Explicit label with htmlFor
<div>
  <label htmlFor="email">Email Address</label>
  <input id="email" type="email" />
</div>

// ✅ GOOD: Implicit label (wrapping)
<label>
  Email Address
  <input type="email" />
</label>

// ❌ BAD: No label
<input type="email" placeholder="Email" />

// ✅ FIX: Add aria-label if visual label not possible
<input type="email" aria-label="Email Address" />
```

---

### Required Fields

```tsx
// ✅ GOOD: Explicit required indicator
<div>
  <label htmlFor="name">
    Name <span aria-label="required">*</span>
  </label>
  <input id="name" type="text" required />
</div>

// ✅ BETTER: Announce "required" to SR
<div>
  <label htmlFor="name">
    Name <span className="text-red-600" aria-label="required">*</span>
  </label>
  <input id="name" type="text" required aria-required="true" />
</div>
```

---

### Error Messages

```tsx
// ✅ GOOD: Error associated with input
<div>
  <label htmlFor="email">Email</label>
  <input
    id="email"
    type="email"
    aria-invalid="true"
    aria-describedby="email-error"
  />
  <span id="email-error" role="alert" className="text-red-600">
    Please enter a valid email address
  </span>
</div>
```

---

## Headings

### Proper Hierarchy

```tsx
// ✅ GOOD: Logical hierarchy
<article>
  <h1>Article Title</h1>
  <section>
    <h2>Section 1</h2>
    <h3>Subsection 1.1</h3>
    <h3>Subsection 1.2</h3>
  </section>
  <section>
    <h2>Section 2</h2>
  </section>
</article>

// ❌ BAD: Skipped levels
<article>
  <h1>Article Title</h1>
  <h3>Section 1</h3> {/* Skipped h2 */}
  <h5>Subsection</h5> {/* Skipped h4 */}
</article>
```

---

### Visual vs. Semantic Headings

```tsx
// ✅ GOOD: Use heading, style with Tailwind
<h2 className="text-lg font-semibold">
  Small but semantic heading
</h2>

// ❌ BAD: Use div styled as heading
<div className="text-3xl font-bold">
  Not a real heading
</div>

// If you MUST break hierarchy for design:
<h3 className="text-5xl font-bold">
  Visually large h3
</h3>
```

---

## Landmarks

### Semantic HTML Landmarks

```tsx
// ✅ GOOD: Semantic landmarks
<header>
  <nav aria-label="Main navigation">{/* Nav */}</nav>
</header>

<main>
  <article>
    <h1>Article Title</h1>
    <section aria-labelledby="features-heading">
      <h2 id="features-heading">Features</h2>
    </section>
  </article>
</main>

<aside aria-label="Related articles">
  {/* Sidebar */}
</aside>

<footer>
  {/* Footer content */}
</footer>
```

---

### Multiple Landmarks of Same Type

```tsx
// When multiple <nav> elements, label them
<header>
  <nav aria-label="Main navigation">
    <a href="/">Home</a>
    <a href="/about">About</a>
  </nav>
</header>

<footer>
  <nav aria-label="Footer navigation">
    <a href="/privacy">Privacy</a>
    <a href="/terms">Terms</a>
  </nav>
</footer>
```

---

## Dynamic Content

### Live Regions

```tsx
// Polite: Wait for user to finish
<div aria-live="polite" role="status">
  {message}
</div>

// Assertive: Interrupt immediately
<div aria-live="assertive" role="alert">
  {errorMessage}
</div>

// Example: Form submission
const [status, setStatus] = useState('')

<form onSubmit={handleSubmit}>
  {/* Form fields */}
  <button type="submit">Submit</button>
</form>

<div aria-live="polite" role="status">
  {status} {/* "Form submitted successfully" */}
</div>
```

---

### Loading States

```tsx
// ✅ GOOD: Announce loading
<div role="status" aria-live="polite">
  {isLoading ? (
    <>
      <Loader className="animate-spin" aria-hidden="true" />
      <span>Loading data...</span>
    </>
  ) : (
    <span className="sr-only">Data loaded</span>
  )}
</div>
```

---

## Tables

### Data Tables

```tsx
// ✅ GOOD: Proper table structure
<table>
  <caption>Employee Directory</caption>
  <thead>
    <tr>
      <th scope="col">Name</th>
      <th scope="col">Department</th>
      <th scope="col">Email</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th scope="row">Jane Smith</th>
      <td>Engineering</td>
      <td>jane@example.com</td>
    </tr>
  </tbody>
</table>

// ❌ BAD: div table
<div className="table">
  <div className="row">
    <div className="cell">Name</div>
  </div>
</div>
```

---

## Icons

### Decorative Icons

```tsx
// ✅ GOOD: Hide from SR, provide text
<button>
  <Check className="h-4 w-4 mr-2" aria-hidden="true" />
  <span>Save</span>
</button>
```

---

### Icon-Only Buttons

```tsx
// ✅ GOOD: aria-label for context
<button aria-label="Close dialog">
  <X className="h-5 w-5" aria-hidden="true" />
</button>

// ✅ GOOD: SR-only text
<button>
  <X className="h-5 w-5" aria-hidden="true" />
  <span className="sr-only">Close dialog</span>
</button>
```

---

## Testing with Screen Readers

### Quick Testing

**macOS (VoiceOver):**
```
Cmd + F5: Enable/disable
Ctrl + Option + Arrow: Navigate
Ctrl + Option + Space: Activate
```

**Windows (NVDA, free):**
```
Ctrl + Alt + N: Start NVDA
Down arrow: Next item
Enter: Activate
```

---

### Testing Checklist

1. **Navigate with SR only:**
   - Can you understand page structure?
   - Are all interactive elements announced?
   - Is there redundant information?

2. **Test forms:**
   - Are labels announced?
   - Are errors announced?
   - Are required fields indicated?

3. **Test dynamic content:**
   - Are loading states announced?
   - Are success/error messages announced?

4. **Test images:**
   - Is alt text descriptive?
   - Are decorative images hidden?

---

## Screen Reader Checklist

Before shipping:

- ✅ All images have alt text (or alt="" if decorative)
- ✅ All form inputs have labels
- ✅ All icon-only buttons have aria-label or SR-only text
- ✅ Decorative icons have aria-hidden="true"
- ✅ Link text is descriptive (no "click here")
- ✅ Heading hierarchy is logical (no skipped levels)
- ✅ One h1 per page
- ✅ Landmarks used (header, nav, main, aside, footer)
- ✅ Multiple same landmarks labeled (aria-label)
- ✅ Live regions for dynamic content (aria-live)
- ✅ Loading states announced (role="status")
- ✅ Error messages associated with inputs (aria-describedby)
- ✅ Tables have caption and proper th/td structure

---

**Last Updated:** November 17, 2025
