# TypeScript Patterns

**Purpose:** TypeScript best practices and patterns for Design Mode components

---

## Strict Mode Configuration

**Always use TypeScript strict mode:**

```json
// tsconfig.json
{
  "compilerOptions": {
    "strict": true,
    "noImplicitAny": true,
    "strictNullChecks": true,
    "strictFunctionTypes": true,
    "strictBindCallApply": true,
    "strictPropertyInitialization": true,
    "noImplicitThis": true,
    "alwaysStrict": true
  }
}
```

---

## Component Props Patterns

### Basic Interface

```tsx
// ✅ GOOD: Exported interface with clear types
export interface ButtonProps {
  children: React.ReactNode
  variant?: 'default' | 'outline' | 'ghost'
  size?: 'sm' | 'md' | 'lg'
  disabled?: boolean
  onClick?: () => void
}

export function Button({
  children,
  variant = 'default',
  size = 'md',
  disabled = false,
  onClick
}: ButtonProps) {
  // ...
}
```

---

### Extending HTML Element Props

```tsx
// ✅ GOOD: Extend native element props
import { ButtonHTMLAttributes } from 'react'

export interface ButtonProps extends ButtonHTMLAttributes<HTMLButtonElement> {
  variant?: 'default' | 'outline' | 'ghost'
  size?: 'sm' | 'md' | 'lg'
}

export function Button({
  children,
  variant = 'default',
  size = 'md',
  className,
  ...props  // Spread remaining HTML attributes
}: ButtonProps) {
  return (
    <button className={cn(baseStyles, className)} {...props}>
      {children}
    </button>
  )
}
```

---

### Discriminated Unions (Type-Safe Variants)

```tsx
// ✅ GOOD: Type-safe variant handling
type CardVariant =
  | { variant: 'simple'; title: string }
  | { variant: 'detailed'; title: string; description: string; image: string }

export interface CardProps {
  config: CardVariant
}

export function Card({ config }: CardProps) {
  if (config.variant === 'simple') {
    return <div>{config.title}</div>
  }

  // TypeScript knows description and image exist here
  return (
    <div>
      <h3>{config.title}</h3>
      <p>{config.description}</p>
      <img src={config.image} alt="" />
    </div>
  )
}
```

---

## Generic Components

```tsx
// ✅ GOOD: Generic component for lists
interface ListProps<T> {
  items: T[]
  renderItem: (item: T) => React.ReactNode
  keyExtractor: (item: T) => string
}

export function List<T>({ items, renderItem, keyExtractor }: ListProps<T>) {
  return (
    <ul>
      {items.map((item) => (
        <li key={keyExtractor(item)}>{renderItem(item)}</li>
      ))}
    </ul>
  )
}

// Usage:
interface User {
  id: string
  name: string
}

<List<User>
  items={users}
  renderItem={(user) => <span>{user.name}</span>}
  keyExtractor={(user) => user.id}
/>
```

---

## Type Inference with zod (Form Validation)

```tsx
import { z } from 'zod'
import { useForm } from 'react-hook-form'
import { zodResolver } from '@hookform/resolvers/zod'

// ✅ GOOD: Schema-first, infer types
const formSchema = z.object({
  name: z.string().min(1, 'Name is required'),
  email: z.string().email('Invalid email'),
  age: z.number().min(18, 'Must be 18+').optional(),
})

// Infer TypeScript type from schema
type FormData = z.infer<typeof formSchema>

export function SignupForm() {
  const {
    register,
    handleSubmit,
    formState: { errors }
  } = useForm<FormData>({
    resolver: zodResolver(formSchema)
  })

  const onSubmit = (data: FormData) => {
    // data is fully typed: { name: string, email: string, age?: number }
    console.log(data.name)  // ✅ TypeScript knows this is string
  }

  return <form onSubmit={handleSubmit(onSubmit)}>{/* ... */}</form>
}
```

---

## Utility Types

### Pick & Omit

```tsx
// ✅ GOOD: Reuse types with Pick/Omit
interface User {
  id: string
  name: string
  email: string
  password: string
  createdAt: Date
}

// Pick only needed fields
type UserProfile = Pick<User, 'id' | 'name' | 'email'>

// Omit sensitive fields
type PublicUser = Omit<User, 'password'>

// Usage in component
interface UserCardProps {
  user: UserProfile  // Only id, name, email
}
```

---

### Partial & Required

```tsx
// ✅ GOOD: Make all props optional
interface FormData {
  name: string
  email: string
  phone: string
}

// All fields optional (for editing)
type PartialFormData = Partial<FormData>
// { name?: string, email?: string, phone?: string }

// All fields required (for submission)
type RequiredFormData = Required<FormData>
```

---

### Record

```tsx
// ✅ GOOD: Type-safe object with string keys
type ColorScale = Record<50 | 100 | 200 | 300 | 400 | 500 | 600 | 700 | 800 | 900 | 950, string>

const primary: ColorScale = {
  50: '#eff6ff',
  100: '#dbeafe',
  // ...all shades required
  950: '#172554',
}
```

---

## As Const (Literal Types)

```tsx
// ✅ GOOD: Const assertions for literal types
const aesthetics = [
  'minimal-clean',
  'bold-colorful',
  'glassmorphism',
  'playful-minimalism'
] as const

// Type: readonly ["minimal-clean", "bold-colorful", "glassmorphism", "playful-minimalism"]
type Aesthetic = typeof aesthetics[number]
// Type: "minimal-clean" | "bold-colorful" | "glassmorphism" | "playful-minimalism"

interface DesignConfig {
  aesthetic: Aesthetic  // Only accepts the 4 values
}
```

---

## Type Guards

```tsx
// ✅ GOOD: Type guard functions
interface SuccessResponse {
  status: 'success'
  data: any
}

interface ErrorResponse {
  status: 'error'
  message: string
}

type ApiResponse = SuccessResponse | ErrorResponse

// Type guard
function isSuccess(response: ApiResponse): response is SuccessResponse {
  return response.status === 'success'
}

// Usage
const response: ApiResponse = await fetchData()

if (isSuccess(response)) {
  console.log(response.data)  // ✅ TypeScript knows data exists
} else {
  console.log(response.message)  // ✅ TypeScript knows message exists
}
```

---

## Avoiding `any`

```tsx
// ❌ BAD: any defeats type safety
function processData(data: any) {
  return data.value  // No error if value doesn't exist!
}

// ✅ GOOD: unknown + type guard
function processData(data: unknown) {
  if (typeof data === 'object' && data !== null && 'value' in data) {
    return (data as { value: string }).value
  }
  throw new Error('Invalid data shape')
}

// ✅ BETTER: Define exact shape
interface DataShape {
  value: string
}

function processData(data: DataShape) {
  return data.value  // ✅ Type safe
}
```

---

## React Node Types

```tsx
// ✅ GOOD: Proper types for children/content
interface ContainerProps {
  children: React.ReactNode  // Anything renderable
  header: React.ReactElement  // Must be a React element
  footer?: React.ReactNode  // Optional
}

// More specific types:
interface IconProps {
  icon: React.ComponentType<{ className?: string }>  // Component type
}

interface RenderPropProps {
  render: (data: User) => React.ReactNode  // Render function
}
```

---

## Ref Types

```tsx
import { useRef, forwardRef } from 'react'

// ✅ GOOD: Typed refs
function Component() {
  const inputRef = useRef<HTMLInputElement>(null)

  const focusInput = () => {
    inputRef.current?.focus()  // Safe with null check
  }

  return <input ref={inputRef} />
}

// ✅ GOOD: forwardRef with types
interface InputProps {
  placeholder?: string
}

export const Input = forwardRef<HTMLInputElement, InputProps>(
  ({ placeholder }, ref) => {
    return <input ref={ref} placeholder={placeholder} />
  }
)

Input.displayName = 'Input'
```

---

## Event Handler Types

```tsx
import { ChangeEvent, FormEvent, MouseEvent } from 'react'

interface FormProps {
  onSubmit: (data: FormData) => void
}

export function Form({ onSubmit }: FormProps) {
  // ✅ GOOD: Properly typed event handlers
  const handleChange = (e: ChangeEvent<HTMLInputElement>) => {
    console.log(e.target.value)
  }

  const handleSubmit = (e: FormEvent<HTMLFormElement>) => {
    e.preventDefault()
    // ...
  }

  const handleClick = (e: MouseEvent<HTMLButtonElement>) => {
    console.log(e.currentTarget)
  }

  return (
    <form onSubmit={handleSubmit}>
      <input onChange={handleChange} />
      <button onClick={handleClick}>Submit</button>
    </form>
  )
}
```

---

## Module Augmentation (Tailwind Types)

```tsx
// ✅ GOOD: Augment module for custom Tailwind config
declare module 'tailwindcss/defaultTheme' {
  interface Theme {
    extend: {
      colors: {
        primary: {
          50: string
          100: string
          // ...
        }
      }
    }
  }
}
```

---

## No Any Checklist

Before shipping TypeScript code:

- ✅ No `any` types (use `unknown` if truly needed)
- ✅ All props interfaces exported
- ✅ Event handlers properly typed
- ✅ No TypeScript errors (`tsc --noEmit`)
- ✅ Generic components use type parameters
- ✅ Form schemas use zod + type inference
- ✅ Refs properly typed
- ✅ Utility types used (Pick, Omit, Partial, etc.)

---

**Last Updated:** November 17, 2025
