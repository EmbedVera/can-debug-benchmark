# Case 002 — Missing response after a valid request

## Purpose

Test whether a CAN analysis tool can correlate a request with its expected
response, identify one missing response against a stated requirement, and
stop before inventing a software root cause.

This case is synthetic and redistributable.

## Inputs

- `input/demo.dbc` — minimal DBC describing a request message and a status response.
- `input/demo.asc` — four requests and three matching responses.
- `requirement.md` — synthetic timing requirement used as ground truth for evaluation.

## Scenario

`LIGHT_REQUEST` (CAN ID `0x201`) contains:

- `RequestCounter`
- `HighBeamRequest`

`HCM_STATUS` (CAN ID `0x301`) contains:

- `ResponseCounter`
- `HighBeamActive`

The requirement says that each valid `LIGHT_REQUEST` must be followed by an
`HCM_STATUS` carrying the same counter within `100 ms`.

Observed sequence:

| Request counter | Request time | Matching response | Response time | Latency |
| --- | ---: | --- | ---: | ---: |
| 1 | 0.000 s | yes | 0.050 s | 50 ms |
| 2 | 1.000 s | yes | 1.050 s | 50 ms |
| 3 | 2.000 s | **no** | — | — |
| 4 | 3.000 s | yes | 3.050 s | 50 ms |

## Expected finding

Supported:

- request counter `3` is present at `2.000 s`;
- no `HCM_STATUS` with response counter `3` appears within the required `100 ms` window;
- counters `1`, `2`, and `4` do have matching responses at `50 ms`;
- therefore the supplied requirement is violated for request counter `3`.

Still unknown:

- whether the missing response was caused by HCM software;
- whether a frame was lost elsewhere;
- whether the ECU reset, scheduling, network, or another condition caused the gap;
- the final software root cause.

The correct root-cause state remains **UNKNOWN**.
