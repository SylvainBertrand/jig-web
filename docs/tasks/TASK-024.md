# TASK-024: Image Panel

## Dependencies: TASK-005, TASK-013, TASK-017
## Wave: 5
## Files: `app/src/panels/image/ImagePanel.tsx`, `app/src/panels/image/ImageOverlays.tsx`

## Instructions
**ImagePanel**: Source dropdown (when multi-source) + topic dropdown (image topics) + overlay dropdown (overlay topics + "None") + show/hide checkbox. Canvas element that renders image at currentTime. Use useCurrentMessage hook. Decode ImageMessage: if rgb8, create ImageData and putImageData. Maintain aspect ratio (object-fit: contain behavior manually via canvas sizing).

**ImageOverlays**: After drawing base image on canvas ctx: BoundingBoxOverlay → strokeRect + label text above box. KeypointOverlay → filled circles + connection lines. SegmentationMaskOverlay → ImageData from classColors at opacity.

## Acceptance Criteria
- [ ] Image updates with timeline
- [ ] Aspect ratio maintained
- [ ] Bounding boxes render with labels
- [ ] Keypoints render with connections
- [ ] Overlay toggle works
