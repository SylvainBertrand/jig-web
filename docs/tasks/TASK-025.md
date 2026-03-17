# TASK-025: Design Polish

## Read First: `docs/DESIGN-SYSTEM.md` (generated in Wave 0)

## Dependencies: All Wave 5 tasks
## Wave: 6
## Files: Edits to `src/styles/globals.css` and style-only changes across components

## Instructions
Make every component look professional. NOT generic Material UI / Bootstrap.

Focus areas:
1. FlexLayout tab styling: rounded-t-md, accent bottom border on active, close button appears on hover only
2. Toolbar: proper spacing, vertical separator lines, icon+text alignment
3. TimelineBar: custom scrub bar (not native range input), transport button sizing
4. Buttons: consistent rounded-md, hover bg-surface-hover, 150ms transitions
5. Modals: backdrop blur, shadow-2xl, smooth visibility transitions
6. Signal chips: colored dot, hover state, smooth × appearance
7. Topic tree: proper indentation (pl per level), drag cursor, hover highlights
8. Empty states: centered, subtle, professional
9. Responsive: toolbar text labels hidden below 768px (icons only)
10. Both themes verified: dark default, light via toggle

**Style-only changes.** Do not modify component logic or data flow.

## Acceptance Criteria
- [ ] Dark theme looks polished (no generic feel)
- [ ] Light theme works across all components
- [ ] FlexLayout chrome matches theme tokens
- [ ] No hardcoded colors in component files
