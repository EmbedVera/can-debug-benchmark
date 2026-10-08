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
  001-shortened-request-pulse/
    README.md
    problem.md
    input/
      demo.asc
    expected.json
```

Future cases may add a DBC, requirement excerpt, code, or other engineering artifacts when they are relevant.

## Current cases

### Case 001 — Shortened request pulse

A synthetic CAN message normally arrives every `0.500 s`. Around one event, the intervals become `0.250 s` and then `0.750 s` before returning to normal.

The benchmark expects a tool to detect the timing anomaly while **not** claiming a component root cause from timing evidence alone.

See [`cases/001-shortened-request-pulse`](cases/001-shortened-request-pulse/).

## Evaluation philosophy

A future scoring harness may evaluate at least four layers:

1. **Parsing correctness** — did the tool read the evidence correctly?
2. **Deterministic correctness** — are timestamps, intervals, values, and counts correct?
3. **Engineering correctness** — does the conclusion match what the evidence can support?
4. **Epistemic discipline** — does the tool preserve UNKNOWN when the root cause is not proven?

The benchmark should penalize hallucinated signals, invented requirements, and unsupported root-cause claims more heavily than incomplete but honest answers.

## Related project

For a minimal deterministic timing analyzer that can run against the first case, see [EmbedVera/can-log-toolkit](https://github.com/EmbedVera/can-log-toolkit).

For evidence-driven CAN issue investigation beyond the public benchmark, see [EmbedVera](https://embedvera.com/).

## 中文说明

这是 EmbedVera 的公开 CAN 调试基准仓库。第一批数据采用合成、可公开分发的案例，重点不是“让 AI 猜中答案”，而是验证工具是否能正确读取证据、计算确定性数据，并在证据不足时保留 UNKNOWN。

## License

A public data/code license has not been selected yet. A license will be added before the first tagged benchmark release.
