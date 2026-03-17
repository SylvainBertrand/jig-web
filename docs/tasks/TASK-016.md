# TASK-016: Timeline Bar

## Dependencies: TASK-004, TASK-006, TASK-013
## Wave: 4
## Files: `app/src/components/TimelineBar.tsx`

## Instructions
Height 56px, sticky bottom. Only visible when data loaded.

Layout: [SkipBack][StepBack][Play/Pause][StepForward][SkipForward] [Speed ▼] | [MM:SS.mmm] [═══●══════════] [MM:SS.mmm] | [Annotate] [ZoomFit]

**Transport buttons**: Use lucide icons (SkipBack, ChevronLeft, Play/Pause, ChevronRight, SkipForward). Call timelineStore actions.

**Playback engine**: When isPlaying, run requestAnimationFrame loop. Each frame: `currentTime += (deltaMs / 1000) * playbackSpeed`. Use useRenderLoop hook. Auto-pause at end.

**Scrub bar**: Custom div-based slider. MouseDown on track → calculate time from position → setCurrentTime. MouseMove while dragging updates continuously. Thumb: 16px circle. Track: h-2. `data-testid="timeline-scrub"`.

**Annotation markers**: For each annotation, render a small colored triangle flag at its position on the scrub bar. Hover shows tooltip with label.

**Multi-source bars**: Thin colored lines below the track showing each source's time extent.

**Time display**: format function: `MM:SS.mmm` where mm is zero-padded minutes, ss is zero-padded seconds, mmm is milliseconds. `data-testid="time-display"`.

**Annotate button**: Flag icon. Click opens inline popover: text input for label + color preset buttons. Enter confirms, adds annotation at currentTime.

**Speed selector**: `<select>` with DEFAULT_PLAYBACK_SPEEDS values. Shows "1x", "2x", etc.

`data-testid="btn-play-pause"`.

## Acceptance Criteria
- [ ] Playback advances time at correct speed
- [ ] Scrub bar responds to click and drag
- [ ] Annotations show as colored markers
- [ ] Time displays are MM:SS.mmm format
- [ ] All data-testid attributes present
