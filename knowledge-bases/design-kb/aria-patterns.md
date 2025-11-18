# ARIA Patterns

**Purpose:** ARIA attribute patterns and best practices for accessible components

---

## Core Principle

**Use semantic HTML FIRST, ARIA only when HTML can't express semantics**

```html
<!-- ✅ GOOD: Semantic HTML (no ARIA needed) -->
<button>Click me</button>
<nav><a href="/">Home</a></nav>

<!-- ❌ BAD: Redundant ARIA -->
<div role="button">Click me</div>
<button role="button">Click me</button>
```

---

## Common ARIA Attributes

### aria-label
**Purpose:** Provides accessible name when visible label isn't present

```tsx
// Icon-only button
<button aria-label="Close dialog">
  <X className="h-4 w-4" aria-hidden="true" />
</button>

// Search button with icon
<button aria-label="Search products">
  <Search className="h-5 w-5" />
</button>
```

---

### aria-labelledby
**Purpose:** References another element as the label

```tsx
// Section labeled by heading
<section aria-labelledby="features-heading">
  <h2 id="features-heading">Key Features</h2>
  {/* Features content */}
</section>

// Modal labeled by title
<div role="dialog" aria-labelledby="dialog-title">
  <h2 id="dialog-title">Confirm Action</h2>
  {/* Dialog content */}
</div>
```

---

### aria-describedby
**Purpose:** References additional descriptive text

```tsx
// Form input with help text
<div>
  <label htmlFor="email">Email</label>
  <input
    id="email"
    type="email"
    aria-describedby="email-help"
  />
  <span id="email-help">We'll never share your email</span>
</div>

// Error message
<div>
  <input
    id="password"
    type="password"
    aria-invalid="true"
    aria-describedby="password-error"
  />
  <span id="password-error" role="alert">
    Password must be at least 8 characters
  </span>
</div>
```

---

### aria-expanded
**Purpose:** Indicates if collapsible content is expanded

```tsx
// Accordion
<button
  aria-expanded={isOpen}
  aria-controls="content-1"
  onClick={() => setIsOpen(!isOpen)}
>
  Show Details
</button>
<div id="content-1" hidden={!isOpen}>
  Content here
</div>

// Dropdown menu
<button
  aria-expanded={menuOpen}
  aria-controls="menu"
  aria-haspopup="true"
>
  Menu
</button>
```

---

### aria-hidden
**Purpose:** Hides element from screen readers (decorative content)

```tsx
// Decorative icons
<div>
  <Check className="h-4 w-4 text-green-600" aria-hidden="true" />
  <span>Task completed</span>
</div>

// Decorative images
<img src="/decoration.svg" alt="" aria-hidden="true" />

// ⚠️ WARNING: Never use aria-hidden on focusable elements
// ❌ BAD:
<button aria-hidden="true">Click me</button>
```

---

### aria-live
**Purpose:** Announces dynamic content changes

```tsx
// Polite announcements (waits for user to pause)
<div aria-live="polite" role="status">
  {statusMessage}
</div>

// Assertive announcements (interrupts immediately)
<div aria-live="assertive" role="alert">
  {errorMessage}
</div>

// Example: Form success message
<div aria-live="polite" role="status">
  {isSubmitted && "Form submitted successfully"}
</div>
```

---

### aria-invalid
**Purpose:** Indicates form field has validation error

```tsx
<input
  id="email"
  type="email"
  aria-invalid={hasError ? "true" : "false"}
  aria-describedby={hasError ? "email-error" : undefined}
/>
{hasError && (
  <span id="email-error" role="alert">
    Invalid email address
  </span>
)}
```

---

### aria-required
**Purpose:** Indicates required form field

```tsx
// Explicit aria-required (when required attr doesn't work)
<div>
  <label htmlFor="name">Name *</label>
  <input
    id="name"
    type="text"
    required
    aria-required="true"
  />
</div>

// Note: HTML5 `required` attribute is often sufficient
<input id="email" type="email" required />
```

---

### aria-current
**Purpose:** Indicates current item in a set (navigation, pagination)

```tsx
// Navigation
<nav>
  <a href="/" aria-current={pathname === '/' ? 'page' : undefined}>
    Home
  </a>
  <a href="/about" aria-current={pathname === '/about' ? 'page' : undefined}>
    About
  </a>
</nav>

// Breadcrumb
<nav aria-label="Breadcrumb">
  <a href="/">Home</a>
  <a href="/products">Products</a>
  <a href="/products/shoes" aria-current="page">Shoes</a>
</nav>
```

---

## ARIA Roles

### role="alert"
**Purpose:** Important, time-sensitive message

```tsx
// Error message
<div role="alert" className="bg-red-50 text-red-900 p-4">
  <AlertCircle className="h-5 w-5" aria-hidden="true" />
  <span>Failed to save changes</span>
</div>
```

---

### role="status"
**Purpose:** Advisory message (not urgent)

```tsx
// Loading indicator
<div role="status" aria-live="polite">
  <Loader className="animate-spin" aria-hidden="true" />
  <span>Loading...</span>
</div>

// Success message
<div role="status">
  <Check aria-hidden="true" />
  <span>Saved</span>
</div>
```

---

### role="dialog"
**Purpose:** Modal dialog

```tsx
<div
  role="dialog"
  aria-labelledby="dialog-title"
  aria-describedby="dialog-description"
  aria-modal="true"
>
  <h2 id="dialog-title">Confirm Delete</h2>
  <p id="dialog-description">
    Are you sure you want to delete this item?
  </p>
  <button>Cancel</button>
  <button>Delete</button>
</div>
```

---

### role="navigation"
**Purpose:** Collection of navigational links (use `<nav>` instead)

```tsx
// ✅ GOOD: Use semantic HTML
<nav aria-label="Main navigation">
  <a href="/">Home</a>
  <a href="/about">About</a>
</nav>

// ❌ BAD: Unnecessary role
<div role="navigation">
  <a href="/">Home</a>
</div>
```

---

### role="button"
**Purpose:** Makes non-button element act like button

```tsx
// ✅ ACCEPTABLE: div as button (when absolutely necessary)
<div
  role="button"
  tabIndex={0}
  onClick={handleClick}
  onKeyDown={(e) => {
    if (e.key === 'Enter' || e.key === ' ') {
      handleClick()
    }
  }}
  aria-label="Toggle menu"
>
  ☰
</div>

// ✅ BETTER: Just use a button
<button onClick={handleClick} aria-label="Toggle menu">
  ☰
</button>
```

---

## Pattern: Expandable Accordion

```tsx
"use client"

import { useState } from 'react'

export function Accordion({ items }) {
  const [openIndex, setOpenIndex] = useState<number | null>(null)

  return (
    <div>
      {items.map((item, index) => (
        <div key={index}>
          <button
            id={`accordion-button-${index}`}
            aria-expanded={openIndex === index}
            aria-controls={`accordion-content-${index}`}
            onClick={() => setOpenIndex(openIndex === index ? null : index)}
            className="w-full text-left p-4 font-semibold"
          >
            {item.title}
          </button>
          <div
            id={`accordion-content-${index}`}
            role="region"
            aria-labelledby={`accordion-button-${index}`}
            hidden={openIndex !== index}
            className="p-4"
          >
            {item.content}
          </div>
        </div>
      ))}
    </div>
  )
}
```

---

## Pattern: Modal Dialog

```tsx
"use client"

import { useEffect, useRef } from 'react'

interface ModalProps {
  isOpen: boolean
  onClose: () => void
  title: string
  children: React.ReactNode
}

export function Modal({ isOpen, onClose, title, children }: ModalProps) {
  const dialogRef = useRef<HTMLDivElement>(null)

  useEffect(() => {
    if (isOpen) {
      // Focus trap: focus first focusable element
      const firstFocusable = dialogRef.current?.querySelector<HTMLElement>(
        'button, [href], input, select, textarea, [tabindex]:not([tabindex="-1"])'
      )
      firstFocusable?.focus()

      // Close on Escape
      const handleEscape = (e: KeyboardEvent) => {
        if (e.key === 'Escape') onClose()
      }
      document.addEventListener('keydown', handleEscape)
      return () => document.removeEventListener('keydown', handleEscape)
    }
  }, [isOpen, onClose])

  if (!isOpen) return null

  return (
    <>
      {/* Backdrop */}
      <div
        className="fixed inset-0 bg-black/50"
        onClick={onClose}
        aria-hidden="true"
      />

      {/* Dialog */}
      <div
        ref={dialogRef}
        role="dialog"
        aria-modal="true"
        aria-labelledby="dialog-title"
        className="fixed top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 bg-white rounded-lg p-6"
      >
        <h2 id="dialog-title" className="text-xl font-bold">
          {title}
        </h2>

        <div className="mt-4">{children}</div>

        <button
          onClick={onClose}
          className="mt-4"
          aria-label="Close dialog"
        >
          Close
        </button>
      </div>
    </>
  )
}
```

---

## Pattern: Tab Panel

```tsx
"use client"

import { useState } from 'react'

interface Tab {
  id: string
  label: string
  content: React.ReactNode
}

export function Tabs({ tabs }: { tabs: Tab[] }) {
  const [activeTab, setActiveTab] = useState(tabs[0].id)

  return (
    <div>
      {/* Tab List */}
      <div role="tablist" className="flex border-b">
        {tabs.map((tab) => (
          <button
            key={tab.id}
            id={`tab-${tab.id}`}
            role="tab"
            aria-selected={activeTab === tab.id}
            aria-controls={`panel-${tab.id}`}
            tabIndex={activeTab === tab.id ? 0 : -1}
            onClick={() => setActiveTab(tab.id)}
            className={activeTab === tab.id ? 'font-bold border-b-2' : ''}
          >
            {tab.label}
          </button>
        ))}
      </div>

      {/* Tab Panels */}
      {tabs.map((tab) => (
        <div
          key={tab.id}
          id={`panel-${tab.id}`}
          role="tabpanel"
          aria-labelledby={`tab-${tab.id}`}
          hidden={activeTab !== tab.id}
          tabIndex={0}
          className="p-4"
        >
          {tab.content}
        </div>
      ))}
    </div>
  )
}
```

---

## Pattern: Combobox (Autocomplete)

```tsx
"use client"

import { useState } from 'react'

export function Combobox({ options }: { options: string[] }) {
  const [value, setValue] = useState('')
  const [isOpen, setIsOpen] = useState(false)
  const [activeIndex, setActiveIndex] = useState(-1)

  const filteredOptions = options.filter((option) =>
    option.toLowerCase().includes(value.toLowerCase())
  )

  return (
    <div>
      <input
        type="text"
        role="combobox"
        aria-expanded={isOpen}
        aria-controls="listbox"
        aria-activedescendant={
          activeIndex >= 0 ? `option-${activeIndex}` : undefined
        }
        value={value}
        onChange={(e) => {
          setValue(e.target.value)
          setIsOpen(true)
        }}
        onFocus={() => setIsOpen(true)}
      />

      {isOpen && (
        <ul id="listbox" role="listbox">
          {filteredOptions.map((option, index) => (
            <li
              key={option}
              id={`option-${index}`}
              role="option"
              aria-selected={activeIndex === index}
              onClick={() => {
                setValue(option)
                setIsOpen(false)
              }}
            >
              {option}
            </li>
          ))}
        </ul>
      )}
    </div>
  )
}
```

---

## ARIA Checklist

Before shipping a component with ARIA:

- ✅ Semantic HTML used first (button, nav, etc.)
- ✅ ARIA only added when HTML can't express semantics
- ✅ No redundant roles (`<button role="button">`)
- ✅ All interactive elements have accessible names
- ✅ aria-labelledby/aria-describedby IDs exist
- ✅ aria-expanded synced with actual state
- ✅ aria-hidden never on focusable elements
- ✅ aria-live regions for dynamic content
- ✅ Form errors use aria-invalid + aria-describedby
- ✅ Modals have aria-modal="true" + focus trap
- ✅ Navigation uses aria-current for active page

---

## Resources

- **WAI-ARIA Authoring Practices:** https://www.w3.org/WAI/ARIA/apg/
- **ARIA in HTML:** https://www.w3.org/TR/html-aria/
- **Inclusive Components:** https://inclusive-components.design/

---

**Last Updated:** November 17, 2025
