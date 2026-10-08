# Case 001 — Shortened request pulse

## Purpose

Test whether a CAN analysis tool can detect a localized message-timing anomaly without overstating what the evidence proves.

## Input

- `input/demo.asc` — synthetic classic CAN traffic for CAN ID `0x123`.

## Known pattern

The dominant interval is `0.500 s`.

Around the event at `1.250 s`, the sequence contains:

- `1.000 -> 1.250 s` = `0.250 s`;
- `1.250 -> 2.000 s` = `0.750 s`;
- then it returns to `0.500 s`.

## Expected reasoning boundary

Supported:

- CAN ID `0x123` has a localized timing deviation.
- The shortened interval begins at the event ending at `1.250 s`.
- A longer interval follows before timing returns to the dominant pattern.

Not supported by this input alone:

- which ECU is faulty;
- why the timing changed;
- whether the observed behavior violates a requirement;
- a final software root cause.

The correct root-cause state for this isolated input is therefore **UNKNOWN**.
