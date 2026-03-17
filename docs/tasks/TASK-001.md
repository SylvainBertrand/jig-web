# TASK-001: Project Configuration

## Dependencies: None
## Wave: 1

## Files to Create
- `app/package.json`
- `app/tsconfig.json`
- `app/vite.config.ts`
- `app/tailwind.config.ts`
- `app/postcss.config.js`
- `app/index.html`

## Skills to Read First
- `.claude/skills/tailwind-theming.md` (for tailwind.config.ts and font links)

## Instructions

Create the `app/` directory and all config files.

**package.json**: Use these EXACT dependencies:
```json
{
  "name": "jig-web",
  "version": "0.1.0",
  "private": true,
  "type": "module",
  "scripts": {
    "dev": "vite",
    "build": "tsc -b && vite build",
    "preview": "vite preview",
    "test": "vitest run",
    "test:watch": "vitest",
    "e2e": "playwright test"
  },
  "dependencies": {
    "react": "^18.3",
    "react-dom": "^18.3",
    "zustand": "^4.5",
    "flexlayout-react": "^0.7",
    "three": "^0.170",
    "@react-three/fiber": "^8.17",
    "@react-three/drei": "^9.114",
    "urdf-loader": "^0.12",
    "uplot": "^1.6",
    "@mcap/browser": "^0.4",
    "@mcap/core": "^2.4",
    "@mcap/support": "^0.5",
    "fuse.js": "^7.0",
    "lucide-react": "^0.460",
    "mathjs": "^13.0",
    "idb-keyval": "^6.2"
  },
  "devDependencies": {
    "typescript": "^5.6",
    "vite": "^6.0",
    "@vitejs/plugin-react": "^4.3",
    "tailwindcss": "^3.4",
    "autoprefixer": "^10.4",
    "postcss": "^8.4",
    "@types/three": "^0.170",
    "@types/react": "^18.3",
    "@types/react-dom": "^18.3",
    "vitest": "^2.1",
    "jsdom": "^25.0",
    "@playwright/test": "^1.49"
  }
}
```

**tsconfig.json**: Strict mode, JSX react-jsx, ES2020 target, module ESNext, moduleResolution bundler. Include `src/**/*.ts` and `src/**/*.tsx`.

**vite.config.ts**: React plugin. No special worker config needed (Vite handles `new Worker(new URL(...))` natively).

**tailwind.config.ts**: Extend colors with CSS variable references per the tailwind-theming skill. Add font family extensions for Inter (sans) and JetBrains Mono (mono).

**postcss.config.js**: Standard tailwindcss + autoprefixer.

**index.html**: Standard Vite HTML. Include Google Fonts links for Inter (400,500,600,700) and JetBrains Mono (400,500). Root div id="root". Title: "Jig — Robotics Visualization Workbench".

## Post-Task
```bash
cd app && npm install
cd app && npx tsc --noEmit  # will fail (no src files yet) — that's OK for Wave 1
```
Update `docs/STATE.md`: mark TASK-001 as done.
```bash
git add -A && git commit -m "TASK-001: Project configuration"
```

## Acceptance Criteria
- [ ] `cd app && npm install` succeeds with no errors
- [ ] All 6 config files exist
- [ ] tsconfig has `"strict": true`
- [ ] tailwind.config extends colors with CSS variable references
- [ ] index.html has Google Fonts links
