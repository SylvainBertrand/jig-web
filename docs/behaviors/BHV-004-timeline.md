# BHV-004: Timeline Playback

## Scenario 1: Play and pause
GIVEN data is loaded
WHEN I press Space
THEN time advances, chart cursor moves, robot animates
WHEN I press Space again
THEN time stops, robot freezes

## Scenario 2: Step
WHEN I press right arrow
THEN time advances by ~10ms and all panels update
WHEN I press left arrow
THEN time goes back by ~10ms

## Scenario 3: Speed change
WHEN I change speed selector to "2x"
THEN time advances roughly twice as fast

## Scenario 4: Seek
WHEN I press Home THEN time jumps to start
WHEN I press End THEN time jumps to end
