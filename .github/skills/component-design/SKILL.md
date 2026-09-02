# Component Design — Accessibility & Responsive SKILL

This skill provides **accessibility patterns (WCAG 2.1 AA), responsive design frameworks, and Storybook conventions** for designing components.

## When to Use This Skill

- Designing UI components or user interactions
- Writing component specs for developers
- Creating Storybook stories
- Accessibility or responsive design concerns

---

## Accessibility (WCAG 2.1 AA)

### 1. Keyboard Navigation

**Rule:** Every interactive element must be accessible via keyboard.

```typescript
// ✅ Component accepts keyboard events
<button 
  onClick={handleClick}
  onKeyDown={(e) => {
    if (e.key === 'Enter' || e.key === ' ') handleClick();
  }}
>
  Subscribe
</button>

// ✅ Focus order makes sense
// ✅ No keyboard trap (user can Tab away from any element)
```

### 2. Screen Reader Support (ARIA)

**Rule:** Interactive elements have semantic meaning for assistive tech.

```typescript
// ✅ Use semantic HTML
<button>Submit</button>  // Not <div onClick>

// ✅ Form field has label
<label htmlFor="email">Email</label>
<input id="email" type="email" />

// ✅ Complex widget needs ARIA
<div
  role="dialog"
  aria-labelledby="dialog-title"
  aria-describedby="dialog-description"
>
  <h2 id="dialog-title">Subscribe</h2>
  <p id="dialog-description">Join our mailing list</p>
</div>

// ✅ Live regions for dynamic content
<div aria-live="polite" aria-atomic="true">
  {message}
</div>
```

### 3. Focus Management

**Rule:** Focus indicator must be visible (can't rely on default).

```css
/* ✅ High contrast focus indicator (4.5:1 minimum) */
button:focus-visible {
  outline: 2px solid hsl(var(--primary));
  outline-offset: 2px;
}

/* ❌ Don't hide focus */
button:focus {
  outline: none; /* BAD */
}
```

### 4. Color Contrast

**Rule:** Text must have 4.5:1 contrast ratio (WCAG AA).

```
Foreground: #333 (dark gray)
Background: #FFF (white)
Contrast: 12.6:1 ✅ (WCAG AAA even)

---

Foreground: #666 (medium gray)
Background: #FFF (white)
Contrast: 3.8:1 ❌ (below 4.5:1)
```

Use tools like [WebAIM Contrast Checker](https://webaim.org/resources/contrastchecker/).

### 5. Zoom & Text Sizing

**Rule:** Page must work at 200% zoom and custom text sizing.

```css
/* ✅ Use relative units */
font-size: 1rem;  /* 16px base */
padding: 1rem;

/* ❌ Fixed pixels don't scale */
font-size: 16px;
padding: 16px;
```

**Test:** Set browser zoom to 200%. Can users still read and interact?

---

## Responsive Design

### Breakpoints

Define your breakpoints once and reuse:

```
Mobile:   320px  (sm)
Tablet:   768px  (md)
Desktop: 1024px  (lg)
Wide:    1440px  (xl)
```

### Mobile-First Approach

```css
/* Mobile first (default) */
.card {
  display: block;
  width: 100%;
}

/* Tablet and up */
@media (min-width: 768px) {
  .card {
    display: grid;
    grid-template-columns: 1fr 1fr;
    width: 50%;
  }
}

/* Desktop and up */
@media (min-width: 1024px) {
  .card {
    width: 33%;
  }
}
```

### Responsive Typography

```css
/* Scale font size with viewport */
body {
  font-size: clamp(1rem, 2vw, 1.25rem);
  /* Minimum 1rem, prefer 2% of viewport, max 1.25rem */
}

/* Absolute sizing (bad for accessibility) */
body {
  font-size: 18px; /* ❌ won't scale with user zoom */
}
```

---

## Component Spec Template

Define a component so developers can build it:

```markdown
# Component: SubscribeForm

## Purpose
Allow users to join the mailing list.

## Props

| Prop | Type | Required | Description |
|------|------|----------|-------------|
| onSubmit | `(email: string) => Promise<void>` | Yes | Callback when form is submitted |
| isLoading | `boolean` | No | Show spinner during submission |
| error | `string \| null` | No | Error message to display |

## States

| State | Appearance | Interaction |
|-------|-----------|------------|
| Default | Empty input, enabled button | User can type and submit |
| Filled | Email entered, enabled button | Submit button highlights on hover |
| Loading | Spinner overlay, disabled button | Button disabled, form frozen |
| Success | "Check your email" message | Form hidden, success shown |
| Error | Error message in red, refocused input | User can correct and retry |

## Accessibility

- [ ] Form has `<form>` tag (not `<div>`)
- [ ] Input has `<label>` (not just placeholder)
- [ ] Error message linked to input via `aria-describedby`
- [ ] Success message announced to screen readers (`aria-live="polite"`)
- [ ] Keyboard accessible (Tab, Enter, Escape)
- [ ] Focus indicator visible (4.5:1 contrast)
- [ ] Color contrast 4.5:1 minimum

## Responsive Breakpoints

| Breakpoint | Layout | Notes |
|-----------|--------|-------|
| 320px (mobile) | Full width, single column | Input and button stack vertically |
| 768px (tablet) | 80% width, centered | Input and button side-by-side |
| 1024px (desktop) | 400px fixed width | Input and button side-by-side |

## Storybook Stories Needed

1. **SubscribeForm/Default** — empty form, ready to use
2. **SubscribeForm/Filled** — email entered, button highlighted
3. **SubscribeForm/Loading** — spinner, disabled
4. **SubscribeForm/Success** — success message
5. **SubscribeForm/Error** — error state, retry
6. **SubscribeForm/Mobile** — responsive at 320px
7. **SubscribeForm/A11y** — test keyboard nav and screen reader
```

---

## Storybook Story Patterns

```typescript
import type { Meta, StoryObj } from '@storybook/react';
import { SubscribeForm } from './SubscribeForm';

const meta: Meta<typeof SubscribeForm> = {
  component: SubscribeForm,
  title: 'Components/SubscribeForm',
  tags: ['autodocs'],
  argTypes: {
    onSubmit: { action: 'submitted' },
    isLoading: { control: 'boolean' },
    error: { control: 'text' },
  },
};

export default meta;
type Story = StoryObj<typeof meta>;

// Default state
export const Default: Story = {
  args: {
    isLoading: false,
    error: null,
  },
};

// Loading state
export const Loading: Story = {
  args: {
    isLoading: true,
    error: null,
  },
};

// Error state
export const Error: Story = {
  args: {
    isLoading: false,
    error: 'Invalid email address',
  },
};

// Mobile viewport
export const Mobile: Story = {
  args: { isLoading: false, error: null },
  parameters: {
    viewport: { defaultViewport: 'mobile1' },
  },
};

// Accessibility test (interactions)
export const A11y: Story = {
  args: { isLoading: false, error: null },
  play: async ({ canvasElement }) => {
    const canvas = within(canvasElement);
    
    // Tab to email input
    await userEvent.tab();
    await expect(canvas.getByRole('textbox')).toBeFocused();
    
    // Tab to submit button
    await userEvent.tab();
    await expect(canvas.getByRole('button')).toBeFocused();
    
    // Enter submits form
    await userEvent.keyboard('{Enter}');
    await expect(canvas.getByText('Check your email')).toBeInTheDocument();
  },
};
```

---

## Design Tokens

Create a consistent design language:

```css
/* CSS Variables (updated with theme) */
:root {
  /* Colors */
  --primary: 217 91% 60%;      /* HSL */
  --primary-rgb: 34 197 94;   /* RGB for rgba() */
  
  --foreground: 0 0% 3.6%;
  --background: 0 0% 100%;
  
  --success: 142 71% 45%;
  --error: 0 84% 60%;
  --warning: 38 92% 50%;
  
  /* Spacing */
  --spacing-xs: 0.25rem;
  --spacing-sm: 0.5rem;
  --spacing-md: 1rem;
  --spacing-lg: 1.5rem;
  --spacing-xl: 2rem;
  
  /* Typography */
  --font-base: -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
  --font-mono: "Menlo", "Monaco", monospace;
  
  /* Border radius */
  --radius-sm: 0.25rem;
  --radius-md: 0.5rem;
  --radius-lg: 0.75rem;
}
```

---

## Common Patterns

### Form Field
```typescript
<div className="field">
  <label htmlFor="email">Email</label>
  <input 
    id="email" 
    type="email"
    aria-describedby="email-error"
  />
  {error && <span id="email-error" className="error">{error}</span>}
</div>
```

### Button States
```typescript
<button 
  disabled={isLoading}
  aria-busy={isLoading}
>
  {isLoading ? 'Loading...' : 'Submit'}
</button>
```

### Responsive Grid
```typescript
<div className="grid" style={{
  display: 'grid',
  gridTemplateColumns: 'repeat(auto-fit, minmax(250px, 1fr))',
  gap: 'var(--spacing-lg)',
}}>
  {items.map(item => <Card key={item.id} {...item} />)}
</div>
```

---

## Validation Checklist

Before handing off to QE:

- [ ] All states defined (default, loading, success, error)
- [ ] Keyboard accessible (Tab order, Enter/Escape)
- [ ] Screen reader friendly (semantic HTML, ARIA)
- [ ] Focus indicator visible (4.5:1 contrast)
- [ ] Color contrast 4.5:1 minimum
- [ ] Responsive at breakpoints (320px, 768px, 1024px, 1440px)
- [ ] Storybook stories cover all states + mobile + a11y
- [ ] Respects `prefers-reduced-motion` (no seizure risk)
- [ ] Works at 200% zoom

---

## Anti-Patterns

❌ **Styled divs instead of semantic HTML** — `<div onClick>` instead of `<button>`  
✅ **Better:** Use semantic elements; they bring built-in a11y

❌ **Placeholder as label** — label hidden, only placeholder visible  
✅ **Better:** Visible label + placeholder for example

❌ **Colors as only differentiator** — red error (colorblind can't see)  
✅ **Better:** Color + icon + text (e.g., ⚠ Error message)

❌ **Fixed width fonts** — doesn't respond to zoom or user settings  
✅ **Better:** Relative units (rem, %, clamp)

❌ **Auto-playing video/animations** — distracting, seizure risk  
✅ **Better:** User-triggered; respect `prefers-reduced-motion`

---

## References

- **WCAG 2.1 AA:** [https://www.w3.org/WAI/WCAG21/quickref](https://www.w3.org/WAI/WCAG21/quickref)
- **Web Accessibility by Google:** [https://www.udacity.com/course/web-accessibility--ud891](https://www.udacity.com/course/web-accessibility--ud891)
- **Storybook Accessibility:** [https://storybook.js.org/docs/writing-stories/testing#accessibility-testing](https://storybook.js.org/docs/writing-stories/testing#accessibility-testing)
- **WebAIM:** [https://webaim.org](https://webaim.org)
