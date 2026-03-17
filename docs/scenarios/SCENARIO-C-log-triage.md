# Scenario C: Quick Log Triage

## User Goal
I received a log from a field test. I want to quickly understand what happened: what topics are in the file, what's the time range, any obvious sensor anomalies.

## Required Layout
- Start with Topic Browser full-width
- Create charts on-demand by double-clicking signals

## Required Interactions
1. Open MCAP file
2. Scan topic list in Topic Browser
3. Double-click a few interesting signals -> new chart tabs auto-created
4. Scrub through the timeline looking for anomalies
5. Annotate 2-3 interesting moments
6. Export annotated region

## What This Tests
- Double-click -> new chart panel works
- Multiple chart tabs can be created dynamically
- Annotation workflow is fast (< 3 actions)
- Export dialog correctly uses view range
