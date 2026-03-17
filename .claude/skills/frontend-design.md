# Frontend Design Principles

Before writing any component code, think through these 4 dimensions:

## 1. Purpose
- Who uses this? (Robotics engineers, researchers)
- Primary task? (Analyzing robot log data, debugging behavior)
- Environment? (Dark labs, long sessions, data-dense screens)

## 2. Tone
- Technical, precise, information-dense
- NOT playful, NOT marketing, NOT consumer-app
- Reference tools: Foxglove Studio, Grafana, VS Code (not Apple, not Stripe)

## 3. Constraints
- React + TypeScript + Tailwind
- Must work at 1280px+ (no mobile)
- Dark-first, light as alternative
- Panels are resizable — components must handle dynamic sizing
- Performance matters — 100Hz data streams

## 4. Differentiation (avoid AI slop)
- NO purple gradients on white
- NO oversized hero sections
- NO rounded-2xl cards with excessive shadows
- NO generic "Get Started" buttons
- YES compact controls (28px buttons, 13px text)
- YES information density (show more data, less chrome)
- YES monospace for all numeric values
- YES border-based depth (not shadow-based)

## Design System Workflow
1. Read `docs/DESIGN-SYSTEM.md` (generated in Wave 0) before any component
2. Apply its spacing, typography, and color rules consistently
3. When in doubt, make it more compact, not more spacious

## Layout Decisions
- Frequent actions: 1 click or keyboard shortcut (drag-drop, Space for play)
- Occasional actions: 2 clicks (dropdown, popover)
- Rare actions: dialog is fine (3+ clicks)
- See UX Priority Map in `docs/SPEC.md`
