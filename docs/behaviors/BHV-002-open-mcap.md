# BHV-002: Open MCAP File

## Scenario 1: Load a valid MCAP file
GIVEN the empty state is showing
WHEN I click "Open MCAP" and select a valid .mcap file
THEN a progress bar appears showing loading progress
AND the progress bar reaches 100% and disappears
AND the Topic Browser shows topics from the file
AND the Timeline bar appears with the file's time range
AND the toolbar shows a colored dot with the filename as tooltip
AND the default panel layout appears

## Scenario 2: Load an invalid file
GIVEN the empty state is showing
WHEN I click "Open MCAP" and select a non-MCAP file
THEN an error notification appears: "Failed to load X: not a valid MCAP file"
AND the empty state remains visible

## Scenario 3: MCAP with unsupported encoding
GIVEN I open an MCAP with CDR-encoded messages
THEN topics appear in the Topic Browser with their names
AND topics with unsupported encoding show a warning indicator
AND a notification says "N topics use unsupported encoding. Scalar data not available for these topics."
