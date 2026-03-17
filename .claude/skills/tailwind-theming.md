# Tailwind CSS Theming Patterns

## CSS Custom Properties Approach (Recommended)

Use CSS custom properties that change based on a class on the root element:

```css
/* globals.css */
@tailwind base;
@tailwind components;
@tailwind utilities;

:root {
  --bg: #0f1117;
  --surface: #1a1d27;
  --surface-hover: #232636;
  --border: #2a2d3a;
  --border-light: #363a4a;
  --text-primary: #e2e4e9;
  --text-secondary: #8b8fa3;
  --text-muted: #555970;
  --accent: #3b82f6;
  --accent-hover: #2563eb;
  --success: #22c55e;
  --warning: #f59e0b;
  --error: #ef4444;
}

.light {
  --bg: #f8f9fb;
  --surface: #ffffff;
  --surface-hover: #f1f3f5;
  --border: #e2e4e9;
  --border-light: #d1d5db;
  --text-primary: #1a1d27;
  --text-secondary: #6b7280;
  --text-muted: #9ca3af;
  --accent: #2563eb;
  --accent-hover: #1d4ed8;
  --success: #16a34a;
  --warning: #d97706;
  --error: #dc2626;
}
```

## Tailwind Config Extension

```typescript
// tailwind.config.ts
import type { Config } from 'tailwindcss';

export default {
  content: ['./index.html', './src/**/*.{ts,tsx}'],
  theme: {
    extend: {
      colors: {
        bg: 'var(--bg)',
        surface: 'var(--surface)',
        'surface-hover': 'var(--surface-hover)',
        border: 'var(--border)',
        'border-light': 'var(--border-light)',
        'text-primary': 'var(--text-primary)',
        'text-secondary': 'var(--text-secondary)',
        'text-muted': 'var(--text-muted)',
        accent: 'var(--accent)',
        'accent-hover': 'var(--accent-hover)',
        success: 'var(--success)',
        warning: 'var(--warning)',
        error: 'var(--error)',
      },
      fontFamily: {
        sans: ['Inter', 'system-ui', '-apple-system', 'sans-serif'],
        mono: ["'JetBrains Mono'", "'Fira Code'", 'monospace'],
      },
    },
  },
  plugins: [],
} satisfies Config;
```

## Usage in Components

```tsx
// Now use Tailwind classes that reference your theme:
<div className="bg-bg text-text-primary">
  <header className="bg-surface border-b border-border h-12">
    <button className="bg-surface hover:bg-surface-hover text-text-secondary rounded-md px-3 py-1.5 transition-colors duration-150">
      Click me
    </button>
  </header>
</div>
```

## Theme Switching in React

```tsx
function App() {
  const theme = useLayoutStore((s) => s.theme);

  return (
    <div className={`${theme === 'light' ? 'light' : ''} w-screen h-screen bg-bg text-text-primary`}>
      <AppShell />
    </div>
  );
}
```

## Common Patterns

```tsx
// Button
<button className="flex items-center gap-2 px-3 py-1.5 rounded-md text-text-secondary hover:bg-surface-hover hover:text-text-primary transition-colors duration-150">

// Dropdown/popover
<div className="absolute z-50 mt-1 bg-surface border border-border rounded-lg shadow-lg overflow-hidden">

// Modal backdrop
<div className="fixed inset-0 z-50 flex items-center justify-center bg-black/50 backdrop-blur-sm">
  <div className="bg-surface border border-border rounded-xl shadow-2xl p-6 max-w-md w-full">

// Input field
<input className="w-full bg-bg border border-border rounded-md px-3 py-2 text-text-primary placeholder:text-text-muted focus:outline-none focus:ring-1 focus:ring-accent" />

// Chip/badge
<span className="inline-flex items-center gap-1.5 px-2 py-0.5 rounded-full text-xs bg-surface-hover text-text-secondary">

// Separator
<div className="w-px h-6 bg-border mx-2" />

// Monospace value
<span className="font-mono text-sm text-text-primary">00:15:23.456</span>
```

## Google Fonts in index.html

```html
<link rel="preconnect" href="https://fonts.googleapis.com" />
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&family=JetBrains+Mono:wght@400;500&display=swap" rel="stylesheet" />
```

## Verified — Standard well-documented APIs. Low hallucination risk.
