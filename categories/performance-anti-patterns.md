# Will My AI Write Slow Code?

> The efficiency-gap category. Papers measuring how far AI-generated code sits from expert-level performance, and the tools that catch the anti-patterns before they ship or confirm them once they do.

BrassCoders treats AI-coder performance anti-patterns as the wedge no security linter covers. The four recurring shapes are quadratic string building, prepend-in-a-loop, nested-loop joins, and unbounded reads, and an AI assistant reaches for them because the prompt described the result rather than the bounds. The papers below measure the efficiency gap; the tools below catch the pattern before merge or confirm it under load.

[← Back to the index](../README.md) · [Live version](https://coppersun.dev/research/performance-anti-patterns/)

Sources last verified August 2026.

## Sources (7)

---

### 📄 EffiBench — Benchmarking the Efficiency of Automatically Generated Code
*Huang, Dong et al., 2024* · [arxiv.org/abs/2402.02037](https://arxiv.org/abs/2402.02037)

BrassCoders treats this as the canonical evidence that AI assistants aim for correctness, not efficiency. EffiBench measures the execution time and memory of LLM-generated solutions against canonical efficient solutions on a set of algorithm problems, and reports that the generated code consumes substantially more of both while still passing the functional tests. Builders who assume a green test means acceptable performance should read this first.

- **What it's good for:** proving correct-but-slow is the default, not the exception.
- **Where BrassCoders draws from it:** the framing of why the four anti-patterns ship undetected in the [performance bugs post](https://coppersun.dev/blog/ai-coder-perf-bugs-in-the-wild/).

---

### 📄 Mercury — A Code Efficiency Benchmark for Code Large Language Models
*Du, Mingzhe et al., 2024* · [arxiv.org/abs/2402.07844](https://arxiv.org/abs/2402.07844)

BrassCoders treats Mercury as the load-bearing reference for scoring code-LLM efficiency rather than pass-rate alone. The benchmark grades generated code on runtime efficiency relative to a distribution of human solutions, which surfaces models that produce correct-but-slow output a pass/fail leaderboard would rank as equal. Builders deciding which assistant to trust on hot-path code should weigh an efficiency score, not only a functional-correctness number.

- **What it's good for:** comparing assistants on efficiency, not just correctness.
- **Where BrassCoders draws from it:** the argument that catch-rate parity with a model still leaves a performance gap a deterministic scanner closes.

---

### 📄 ENAMEL — How Efficient Is LLM-Generated Code? A Rigorous & High-Standard Benchmark
*Qiu, Ruizhong et al., 2024* · [arxiv.org/abs/2406.06647](https://arxiv.org/abs/2406.06647)

BrassCoders treats this as the structural proof that frontier models still trail expert-level efficiency. The paper introduces the eff@k metric against an expert-built reference, then shows leading LLMs reach only a fraction of expert efficiency even when their output is functionally correct. Builders citing the gap between AI correctness and AI efficiency should use this as the rigorous source.

- **What it's good for:** a defensible number on the AI efficiency gap.
- **Where BrassCoders draws from it:** the claim that you cannot prompt your way to fast code; you need a deterministic check.

---

### 🔧 radon
*rubik/radon · Python · ~2k stars* · [github.com/rubik/radon](https://github.com/rubik/radon)

BrassCoders bundles radon as the complexity-metrics scanner in the performance layer. radon computes cyclomatic complexity and a maintainability index per function, which flags the over-nested, inline-expanded blocks an AI assistant produces when it answers a prompt in one function instead of decomposing it. Builders who want a number on how tangled the AI made a change should run radon before review.

- **What it's good for:** per-function complexity and maintainability scores.
- **Where BrassCoders draws from it:** one of the 12 bundled scanners; feeds the performance-intelligence findings.

---

### 🔧 py-spy
*benfred/py-spy · Python · 15k+ stars* · [github.com/benfred/py-spy](https://github.com/benfred/py-spy)

BrassCoders bundles py-spy as the sampling profiler for runtime validation. py-spy attaches to a running Python process and samples its call stack with no code changes and no restart, so a quadratic hotspot shows up as measured wall-clock time instead of a static guess. Builders who want to confirm a flagged anti-pattern actually costs time under real load should profile with py-spy.

- **What it's good for:** zero-instrumentation production profiling.
- **Where BrassCoders draws from it:** the runtime-validation step that turns a static finding into a measured cost.

---

### 🔧 Scalene
*plasma-umass/scalene · Python · 13k+ stars* · [github.com/plasma-umass/scalene](https://github.com/plasma-umass/scalene)

BrassCoders treats Scalene as the high-resolution profiler builders should reach for when a hotspot is worth dissecting. Scalene separates CPU, GPU, and memory time at line granularity and attributes cost to Python versus native code, which tells you whether a slow loop is compute-bound or allocation-bound before you rewrite it. Builders tuning a confirmed hotspot should profile with Scalene to aim the fix.

- **What it's good for:** line-level CPU and memory attribution.
- **Where BrassCoders draws from it:** the recommended deep-profiling step after a finding is confirmed costly.

---

### 🧪 BrassCoders AI-Coder-Bugs Corpus
*Copper Sun Brass · Apache 2.0 · reproducible* · [github.com/CopperSunDev/brasscoders](https://github.com/CopperSunDev/brasscoders/tree/main/cli/docs/benchmarks/ai-coder-bugs)

BrassCoders treats its own AI-coder-bugs corpus as the reproducible benchmark for this category. On twelve AI-generated Python files, BrassCoders caught all four planted performance anti-patterns while Bandit, Semgrep, and Pylint each caught none, and a frontier model matched the result only when explicitly asked to review. Builders who want to check the claim can run the committed corpus and runner themselves.

- **What it's good for:** a reproducible head-to-head on the exact patterns AI assistants introduce.
- **Where BrassCoders draws from it:** the published [benchmark write-up](https://coppersun.dev/blog/ai-coder-bug-benchmark/) and the [Bandit-coverage post](https://coppersun.dev/blog/why-bandit-misses-ai-coder-bugs/).

## FAQ

### What are AI-coder performance anti-patterns?

They are the inefficient code shapes AI assistants generate when a prompt describes a result but not its scale. Four recur most: string concatenation in a loop (O(N²) memory copying), list.insert(0) in a loop (O(N²) shifting), nested loops used as a join where a dict lookup would be O(N), and unbounded while-True reads with no size cap or timeout. Each one runs fine on small inputs and degrades at volume.

### Why do AI assistants write inefficient code?

They aim for the stated requirement, which is almost always correctness on the example input. A prompt that says "export these records to CSV" is satisfied by code that works on ten rows; nothing in the prompt asks for the version that holds up at a hundred thousand. Research benchmarks (EffiBench, Mercury, ENAMEL) measure this gap and find generated code reaches a fraction of expert efficiency even when it passes every test.

### Can I just profile the code instead of scanning for these patterns?

Profiling and scanning catch the problem at different stages. A profiler like py-spy or Scalene needs a running workload and a large enough input to make the hotspot show up, which usually means production. A deterministic pre-merge scan flags the anti-pattern from the source before it ships. The reliable workflow is both: scan to catch it early, profile to confirm and tune what remains.

### Does a passing unit test mean the code is fast enough?

No. A unit test checks correctness, almost always on a small input, where an O(N²) loop and an O(N) loop are indistinguishable. The performance bug only appears when the input grows, which a small test never exercises. This is exactly why these bugs survive review and ship: the code is correct, the test is green, and the cost is invisible until volume.

---

*Generated from the canonical research config. CC BY 4.0. [Live version](https://coppersun.dev/research/performance-anti-patterns/) · [Install BrassCoders](https://coppersun.dev/install/).*
