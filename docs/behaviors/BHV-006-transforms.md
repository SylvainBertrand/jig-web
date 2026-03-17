# BHV-006: Chart Transforms

## Scenario 1: Derivative
GIVEN position[0] is on the chart
WHEN I click f(x), select derivative, select position[0], click Apply
THEN a new trace appears that looks like the derivative

## Scenario 2: Expression
GIVEN position[0] and position[1] are on the chart
WHEN I use expression editor with "a - b"
THEN a new trace showing the difference appears
