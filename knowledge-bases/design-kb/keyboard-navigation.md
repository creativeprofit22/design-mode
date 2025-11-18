# Keyboard Navigation

**Purpose:** Keyboard accessibility patterns and best practices

---

## Core Requirements

All interactive elements MUST be:
1. **Focusable** - Accessible via Tab key
2. **Activatable** - Respond to Enter/Space
3. **Navigable** - Logical tab order
4. **Escapable** - Modals close with Escape

---

## Tab Order

### Logical Flow

Tab order should follow visual flow: **top → bottom, left → right**

```tsx
// ✅ GOOD: Natural DOM order
<nav>
  <a href="/">Home</a>      {/* Tab 1 */}
  <a href="/about">About</a> {/* Tab 2 */}
  <a href="/contact">Contact</a> {/* Tab 3 */}
</nav>

// ❌ BAD: CSS changes visual order, but tab order stays DOM order
<nav className="flex flex-col-reverse">
  <a href="/contact">Contact</a> {/* Still Tab 1 visually last */}
  <a href="/about">About</a>
  <a href="/">Home</a>
</nav>
```

---

### tabIndex Values

```tsx
// tabIndex="0" - Include in natural tab order
<div
  tabIndex={0}
  role="button"
  onClick={handleClick}
>
  Custom button
</div>

// tabIndex="-1" - Exclude from tab order (but can be focused programmatically)
<div tabIndex={-1} ref={focusableRef}>
  Programmatically focusable
</div>

// ⚠️ tabIndex > 0 - AVOID (breaks natural tab order)
// ❌ BAD:
<button tabIndex={1}>First</button>
<button tabIndex={2}>Second</button>
```

---

## Focus Indicators

### Visible Focus Styles (MANDATORY)

```tsx
// ✅ GOOD: Tailwind focus styles
<Button className="
  focus:outline-none
  focus-visible:ring-2
  focus-visible:ring-primary
  focus-visible:ring-offset-2
">
  Click me
</Button>

// ✅ GOOD: Custom focus
<button className="
  focus:outline-none
  focus-visible:outline-2
  focus-visible:outline-offset-2
  focus-visible:outline-blue-600
">
  Custom focus
</button>

// ❌ BAD: Hidden focus (WCAG failure)
<button className="focus:outline-none">
  No focus indicator
</button>
```

---

### Focus Styles for Dark Mode

```tsx
// ✅ GOOD: Visible in both modes
<button className="
  focus-visible:ring-2
  focus-visible:ring-primary
  focus-visible:ring-offset-2
  focus-visible:ring-offset-white
  dark:focus-visible:ring-offset-slate-950
">
  Button
</button>
```

---

## Keyboard Shortcuts Reference

| Key | Element | Action |
|-----|---------|--------|
| **Tab** | All focusable | Move focus forward |
| **Shift+Tab** | All focusable | Move focus backward |
| **Enter** | Links, buttons | Activate |
| **Space** | Buttons, checkboxes | Activate/toggle |
| **Escape** | Modals, menus | Close/cancel |
| **Arrow keys** | Menus, tabs, radios | Navigate items |
| **Home** | Lists, text fields | First item/start |
| **End** | Lists, text fields | Last item/end |

---

## Pattern: Skip Links

**Purpose:** Allow keyboard users to skip to main content

```tsx
// Add to layout (before main navigation)
<a
  href="#main"
  className="
    sr-only
    focus:not-sr-only
    focus:absolute
    focus:top-4
    focus:left-4
    focus:z-50
    focus:px-4
    focus:py-2
    focus:bg-primary
    focus:text-primary-foreground
    focus:rounded
  "
>
  Skip to content
</a>

<nav>{/* Navigation */}</nav>

<main id="main" tabIndex={-1}>
  {/* Main content */}
</main>
```

---

## Pattern: Modal Focus Trap

**Purpose:** Keep focus inside modal, return to trigger on close

```tsx
"use client"

import { useEffect, useRef } from 'react'

export function Modal({ isOpen, onClose, children }) {
  const modalRef = useRef<HTMLDivElement>(null)
  const previousFocusRef = useRef<HTMLElement | null>(null)

  useEffect(() => {
    if (isOpen) {
      // Store element that opened modal
      previousFocusRef.current = document.activeElement as HTMLElement

      // Focus first focusable element in modal
      const focusable = modalRef.current?.querySelectorAll<HTMLElement>(
        'button, [href], input, select, textarea, [tabindex]:not([tabindex="-1"])'
      )
      focusable?.[0]?.focus()

      // Trap focus inside modal
      const handleTab = (e: KeyboardEvent) => {
        if (e.key !== 'Tab' || !focusable) return

        const firstFocusable = focusable[0]
        const lastFocusable = focusable[focusable.length - 1]

        if (e.shiftKey && document.activeElement === firstFocusable) {
          e.preventDefault()
          lastFocusable.focus()
        } else if (!e.shiftKey && document.activeElement === lastFocusable) {
          e.preventDefault()
          firstFocusable.focus()
        }
      }

      // Close on Escape
      const handleEscape = (e: KeyboardEvent) => {
        if (e.key === 'Escape') onClose()
      }

      document.addEventListener('keydown', handleTab)
      document.addEventListener('keydown', handleEscape)

      return () => {
        document.removeEventListener('keydown', handleTab)
        document.removeEventListener('keydown', handleEscape)
      }
    } else {
      // Return focus to trigger element
      previousFocusRef.current?.focus()
    }
  }, [isOpen, onClose])

  if (!isOpen) return null

  return (
    <div
      ref={modalRef}
      role="dialog"
      aria-modal="true"
      className="fixed inset-0 z-50"
    >
      {children}
    </div>
  )
}
```

---

## Pattern: Dropdown Menu

```tsx
"use client"

import { useState, useRef, useEffect } from 'react'

export function Dropdown() {
  const [isOpen, setIsOpen] = useState(false)
  const [activeIndex, setActiveIndex] = useState(-1)
  const buttonRef = useRef<HTMLButtonElement>(null)
  const menuRef = useRef<HTMLDivElement>(null)

  const items = ['Profile', 'Settings', 'Sign out']

  const handleKeyDown = (e: React.KeyboardEvent) => {
    if (!isOpen) {
      if (e.key === 'Enter' || e.key === ' ' || e.key === 'ArrowDown') {
        e.preventDefault()
        setIsOpen(true)
        setActiveIndex(0)
      }
      return
    }

    switch (e.key) {
      case 'Escape':
        e.preventDefault()
        setIsOpen(false)
        buttonRef.current?.focus()
        break

      case 'ArrowDown':
        e.preventDefault()
        setActiveIndex((prev) => (prev + 1) % items.length)
        break

      case 'ArrowUp':
        e.preventDefault()
        setActiveIndex((prev) => (prev - 1 + items.length) % items.length)
        break

      case 'Home':
        e.preventDefault()
        setActiveIndex(0)
        break

      case 'End':
        e.preventDefault()
        setActiveIndex(items.length - 1)
        break

      case 'Enter':
      case ' ':
        e.preventDefault()
        // Activate current item
        console.log('Activated:', items[activeIndex])
        setIsOpen(false)
        break
    }
  }

  useEffect(() => {
    if (isOpen && activeIndex >= 0) {
      const activeItem = menuRef.current?.querySelector(
        `[data-index="${activeIndex}"]`
      ) as HTMLElement
      activeItem?.focus()
    }
  }, [activeIndex, isOpen])

  return (
    <div className="relative">
      <button
        ref={buttonRef}
        aria-haspopup="true"
        aria-expanded={isOpen}
        onKeyDown={handleKeyDown}
        onClick={() => setIsOpen(!isOpen)}
      >
        Menu
      </button>

      {isOpen && (
        <div
          ref={menuRef}
          role="menu"
          className="absolute mt-2 bg-white shadow-lg rounded"
        >
          {items.map((item, index) => (
            <button
              key={item}
              role="menuitem"
              data-index={index}
              tabIndex={-1}
              className={activeIndex === index ? 'bg-slate-100' : ''}
              onClick={() => {
                console.log('Clicked:', item)
                setIsOpen(false)
                buttonRef.current?.focus()
              }}
            >
              {item}
            </button>
          ))}
        </div>
      )}
    </div>
  )
}
```

---

## Pattern: Custom Button (div/span as button)

```tsx
// When you MUST use div as button (avoid if possible)
function CustomButton({ children, onClick }) {
  const handleKeyDown = (e: React.KeyboardEvent) => {
    // Activate on Enter or Space
    if (e.key === 'Enter' || e.key === ' ') {
      e.preventDefault()
      onClick?.()
    }
  }

  return (
    <div
      role="button"
      tabIndex={0}
      onClick={onClick}
      onKeyDown={handleKeyDown}
      className="cursor-pointer focus:outline-none focus-visible:ring-2"
    >
      {children}
    </div>
  )
}

// ✅ BETTER: Just use a button
function BetterButton({ children, onClick }) {
  return (
    <button
      onClick={onClick}
      className="focus:outline-none focus-visible:ring-2"
    >
      {children}
    </button>
  )
}
```

---

## Pattern: Tab Navigation

```tsx
"use client"

import { useState, useRef } from 'react'

export function Tabs({ tabs }) {
  const [activeTab, setActiveTab] = useState(0)
  const tabsRef = useRef<(HTMLButtonElement | null)[]>([])

  const handleKeyDown = (e: React.KeyboardEvent, index: number) => {
    let newIndex = index

    switch (e.key) {
      case 'ArrowRight':
        e.preventDefault()
        newIndex = (index + 1) % tabs.length
        break

      case 'ArrowLeft':
        e.preventDefault()
        newIndex = (index - 1 + tabs.length) % tabs.length
        break

      case 'Home':
        e.preventDefault()
        newIndex = 0
        break

      case 'End':
        e.preventDefault()
        newIndex = tabs.length - 1
        break

      default:
        return
    }

    setActiveTab(newIndex)
    tabsRef.current[newIndex]?.focus()
  }

  return (
    <div>
      <div role="tablist">
        {tabs.map((tab, index) => (
          <button
            key={tab.id}
            ref={(el) => (tabsRef.current[index] = el)}
            role="tab"
            aria-selected={activeTab === index}
            aria-controls={`panel-${tab.id}`}
            tabIndex={activeTab === index ? 0 : -1}
            onClick={() => setActiveTab(index)}
            onKeyDown={(e) => handleKeyDown(e, index)}
          >
            {tab.label}
          </button>
        ))}
      </div>

      {tabs.map((tab, index) => (
        <div
          key={tab.id}
          id={`panel-${tab.id}`}
          role="tabpanel"
          hidden={activeTab !== index}
          tabIndex={0}
        >
          {tab.content}
        </div>
      ))}
    </div>
  )
}
```

---

## Testing Keyboard Navigation

### Manual Testing Checklist

1. **Tab through entire page:**
   - All interactive elements focusable?
   - Focus order logical?
   - Focus indicators visible?

2. **Test each component:**
   - Buttons activate on Enter/Space?
   - Links activate on Enter?
   - Modals close on Escape?
   - Dropdowns navigate with arrows?

3. **Test focus management:**
   - Modal traps focus?
   - Modal returns focus on close?
   - Skip links work?

4. **Test in dark mode:**
   - Focus indicators visible?
   - Contrast sufficient?

---

## Keyboard Navigation Checklist

Before shipping:

- ✅ All interactive elements focusable (Tab key)
- ✅ Focus indicators visible (2px ring minimum)
- ✅ Focus visible in dark mode
- ✅ Logical tab order (follows visual flow)
- ✅ No tabIndex > 0 (breaks natural order)
- ✅ Skip link present (before navigation)
- ✅ Modals trap focus
- ✅ Modals close on Escape
- ✅ Modals return focus on close
- ✅ Dropdowns navigate with arrow keys
- ✅ Tabs navigate with arrow keys
- ✅ Custom buttons respond to Enter/Space
- ✅ No keyboard traps (can exit all components)

---

**Last Updated:** November 17, 2025
