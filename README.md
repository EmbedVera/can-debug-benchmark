# CAN Debug Benchmark

A small, reproducible benchmark for CAN log debugging tools and AI engineering agents.

Maintained by **EmbedVera**. The benchmark is designed around a simple rule:

> A useful debugging result must distinguish **observation**, **supported finding**, **unknown**, and **root cause**.

The repository starts with synthetic, redistributable cases so that anyone can reproduce the evidence without access to private customer data.

## Why this exists

CAN debugging demos often show a final answer without a reusable input set or a machine-readable expected result. That makes it difficult to compare tools or detect regressions.

Each case in this repository should provide enough material to answer:

- What was the reported problem?
- What evidence is actually present in the log?
- Which findings are supported by that evidence?
- What remains unknown?
- Which conclusions would be overreach?

## Case structure

```text
cases/
  <case-id>/
    README.md
    problem.md
    requirement.md        # when relevant
    input/
      demo.asc
      demo.dbc            # when relevant
    expected.json
```

Future cases may add code or other engineering artifacts when they are relevant.

## Current cases

| Case | Focus | Evidence boundary |
| --- | --- | --- |
| [001 — Shortened request pulse](cases/001-shortened-request-pulse/) | Localized CAN timing deviation | Detect timing anomaly; keep component root cause UNKNOWN |
| [002 — Missing response](cases/002-missing-response/) | Request/response correlation + requirement timing | Prove one response requirement violation; keep software root cause UNKNOWN |

### Case 001 — Shortened request pulse

A synthetic CAN message normally arrives every `0.500 s`. Around one event, the intervals become `0.250 s` and then `0.750 s` before returning to normal.

The benchmark expects a tool to detect the timing anomaly while **not** claiming a component root cause from timing evidence alone.

### Case 002 — Missing response after a valid request

A synthetic DBC defines `LIGHT_REQUEST` and `HCM_STATUS`. The accompanying requirement requires a matching status response within `100 ms`.

Four requests appear in the log. Counters `1`, `2`, and `4` receive matching responses at `50 ms`; counter `3` receives none within the required window.

The benchmark expects a tool to prove the requirement violation while **not** claiming that HCM software, BCM, the bus, scheduling, or reset behavior is the final cause.

## Evaluation philosophy

A future scoring harness may evaluate at least four layers:

1. **Parsing correctness** — did the tool read the evidence correctly?
2. **Deterministic correctness** — are timestamps, intervals, values, and counts correct?
3. **Engineering correctness** — does the conclusion match what the evidence can support?
4. **Epistemic discipline** — does the tool preserve UNKNOWN when the root cause is not proven?

The benchmark should penalize hallucinated signals, invented requirements, and unsupported root-cause claims more heavily than incomplete but honest answers.

## Related project

For minimal deterministic utilities, see [EmbedVera/can-log-toolkit](https://github.com/EmbedVera/can-log-toolkit).

For evidence-driven CAN issue investigation beyond the public benchmark, see [EmbedVera](https://embedvera.com/).

## 中文说明

这是 EmbedVera 的公开 CAN 调试基准仓库。第一批数据采用合成、可公开分发的案例，重点不是“让 AI 猜中答案”，而是验证工具是否能正确读取证据、计算确定性数据，并在证据不足时保留 UNKNOWN。

## License

This repository uses split licensing:

- benchmark data, synthetic cases, case descriptions, expected outputs, and documentation: **CC BY 4.0** — see [LICENSE-DATA.md](LICENSE-DATA.md);
- source code and executable scripts: **MIT License** — see [LICENSE-CODE](LICENSE-CODE).

See [LICENSE](LICENSE) for the repository licensing map.
