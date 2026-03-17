# Scenario D: Multi-Log Comparison

## User Goal
I ran the same trajectory twice with different controller gains. I want to overlay the joint positions from both runs to compare tracking performance.

## Required Layout
- Chart: signals from both files overlaid

## Required Interactions
1. Open MCAP file #1
2. Open MCAP file #2
3. Drag position[0] from source 1 to chart
4. Drag position[0] from source 2 to SAME chart
5. Both traces visible with distinct source-colored lines
6. Timeline shows both files' time extents

## What This Tests
- Multi-log opening works
- Source colors are distinct and consistent
- Same-field from different sources can be overlaid
- Topic Browser correctly separates sources
