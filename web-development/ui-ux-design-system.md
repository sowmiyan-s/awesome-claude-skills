---
name: ui-ux-design-system
description: Build scalable, accessible (WCAG 2.1 AAA), token-driven design systems and modern UI component architectures with design tokens, fluid responsive typography, accessible keyboard navigation, micro-interactions, dark/light theme switching, and modular component primitives. Use this skill when creating component libraries, establishing design tokens, refactoring UI architecture, ensuring web accessibility, or engineering polished interaction design.
---

# UI/UX Design System & Component Engineer

A master-level skill for crafting cohesive, scalable, accessible design systems and state-of-the-art UI component libraries. Blends deep visual design principles with engineering rigor.

---

## 1. Design Token Architecture

Structure design tokens systematically using a 3-tier hierarchy: **Global $\rightarrow$ Semantic $\rightarrow$ Component**.

### Example Token Hierarchy (CSS Custom Properties)
```css
:root {
  /* 1. Global / Primitive Tokens (Raw values) */
  --primitive-slate-50: #f8fafc;
  --primitive-slate-900: #0f172a;
  --primitive-indigo-500: #6366f1;
  --primitive-indigo-600: #4f46e5;
  --primitive-radius-sm: 4px;
  --primitive-radius-md: 8px;
  --primitive-radius-lg: 16px;

  /* 2. Semantic Tokens (Intent-based mapping) */
  --color-bg-canvas: var(--primitive-slate-50);
  --color-bg-surface: #ffffff;
  --color-text-primary: var(--primitive-slate-900);
  --color-text-muted: #64748b;
  --color-brand-primary: var(--primitive-indigo-500);
  --color-brand-primary-hover: var(--primitive-indigo-600);
  --color-border-subtle: #e2e8f0;
  
  --elevation-1: 0 1px 3px 0 rgb(0 0 0 / 0.1), 0 1px 2px -1px rgb(0 0 0 / 0.1);
  --elevation-2: 0 4px 6px -1px rgb(0 0 0 / 0.1), 0 2px 4px -2px rgb(0 0 0 / 0.1);
  --elevation-3: 0 10px 15px -3px rgb(0 0 0 / 0.1), 0 4px 6px -4px rgb(0 0 0 / 0.1);

  --font-family-sans: 'Inter', system-ui, -apple-system, sans-serif;
  --font-family-mono: 'JetBrains Mono', monospace;
  --font-size-base: clamp(1rem, 0.95rem + 0.25vw, 1.125rem);
}

[data-theme='dark'] {
  --color-bg-canvas: #090d16;
  --color-bg-surface: #111827;
  --color-text-primary: #f8fafc;
  --color-text-muted: #94a3b8;
  --color-border-subtle: #1e293b;
  --elevation-1: 0 1px 3px 0 rgb(0 0 0 / 0.4);
}
```

---

## 2. Accessibility (a11y) & WCAG 2.1 AA Checklist

1. **Color Contrast**: Minimum contrast ratio of 4.5:1 for normal text and 3:1 for large text/UI icons.
2. **Keyboard Focus States**: Never remove `outline: none` without providing a distinct `:focus-visible` styling:
   ```css
   :focus-visible {
     outline: 2px solid var(--color-brand-primary);
     outline-offset: 2px;
   }
   ```
3. **ARIA & Accessible Roles**:
   - Use native HTML elements (`<button>`, `<dialog>`, `<input>`, `<details>`) before building custom ARIA widgets.
   - For custom interactive components, supply proper `aria-expanded`, `aria-controls`, `aria-haspopup`, `aria-describedby`, and `role`.
4. **Reduced Motion**: Respect user OS preferences:
   ```css
   @media (prefers-reduced-motion: reduce) {
     *, ::before, ::after {
       animation-duration: 0.01ms !important;
       transition-duration: 0.01ms !important;
     }
   }
   ```

---

## 3. Component Architecture & Composition Pattern

- Build headless/composable primitives using compound component patterns:
  - `Dialog` $\rightarrow$ `Dialog.Trigger`, `Dialog.Portal`, `Dialog.Overlay`, `Dialog.Content`, `Dialog.Title`, `Dialog.Close`.
- Enforce clear prop ergonomics:
  - Variant props (`variant="primary | secondary | outline | ghost | danger"`).
  - Size props (`size="sm | md | lg"`).
  - State props (`isLoading`, `isDisabled`, `isSelected`).
- Avoid prop-drilling by managing complex local component state via React Context or atomic signals.

---

## 4. Micro-Interactions & Animation Guidelines

1. **Duration & Easing**:
   - Micro-interactions (hover, focus, button tap): `100ms - 200ms` with `ease-out`.
   - Entrances & Modals: `250ms - 350ms` with spring or `cubic-bezier(0.16, 1, 0.3, 1)`.
   - Exits: `150ms - 200ms` with `ease-in`.
2. **Transform & Opacity Only**:
   - Only animate `transform` and `opacity` to maintain smooth 60fps/120fps hardware acceleration on the compositor thread.
   - Avoid animating layout-triggering properties (`width`, `height`, `margin`, `padding`, `top`, `left`).
