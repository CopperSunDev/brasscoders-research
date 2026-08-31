# What Does Coding At AI Speed Do To Security And Quality?

> Vibe coding — fully AI-driven authorship where the developer vibe-checks the output and barely reads it — removes the review step that was already failing to catch security issues. The canonical evidence, the practitioner warnings, and what BrassCoders's own data shows.

BrassCoders traces vibe coding directly to Andrej Karpathy's February 2025 post: the developer fully surrenders authorship to the AI, vibe-checks the output, and ships it. The Perry et al. CCS 2023 study shows what that costs: developers using an AI assistant wrote significantly more insecure code than those without one — and were more likely to believe it was secure. Vibe coding further compresses the review step Perry's participants still had. BrassCoders's own N=15 AI-code-findings corpus confirms the field result: real security issues in 9 of 15 AI-generated Python files, with zero generation-time warnings.

[← Back to the index](../README.md) · [Live version](https://coppersun.dev/research/vibe-coding-velocity-risk/)

Sources last verified August 2026.

## Sources (3)

---

### 📊 Karpathy 2025 — Vibe Coding
*Andrej Karpathy, X/Twitter, February 2, 2025* · [x.com/@karpathy](https://x.com/karpathy/status/1886192184808149186)

BrassCoders traces the practice directly to Andrej Karpathy's February 2025 post defining vibe coding as fully AI-driven authorship — the developer vibe-checks the output, barely reads it. The published N=15 AI-code-findings corpus shows what that produces: real security issues in 9 of 15 AI-generated Python files, with zero generation-time warnings from the model. The 30-second pre-commit scan BrassCoders provides is the gate that vibe coding removes.

- **What it's good for:** the origin and definition of vibe coding as fully AI-driven authorship.
- **Where BrassCoders draws from it:** the framing BrassCoders uses to explain why speed alone is not a safe production strategy for AI-generated code.

---

### 📄 Perry et al. 2023 — Do Users Write More Insecure Code With AI Assistants?
*Neil Perry, Megha Srivastava, Deepak Kumar, Dan Boneh — ACM CCS 2023* · [dl.acm.org](https://dl.acm.org/doi/10.1145/3576915.3623157)

BrassCoders treats this CCS study as the control-group evidence for the vibe-coding risk. Perry et al. found that participants using AI assistants wrote significantly more insecure code than those without — higher frequency of security vulnerabilities across injection, authorization, and cryptographic categories. Vibe coding, which further compresses the review step Perry's participants had, moves the risk in the same direction. The mechanism matches: the model optimizes for code that satisfies the prompt, not code that is safe on attacker input.

- **What it's good for:** the empirical effect of AI assistance on developer security outcomes.
- **Where BrassCoders draws from it:** also indexed in the [weak cryptography](https://coppersun.dev/research/weak-cryptography/) category for the crypto-specific findings; cited here for the broader insecurity effect across all task types.

---

### 📊 Willison 2024 — Things I Worry About With AI-Assisted Coding
*Simon Willison, simonwillison.net, 2024* · [simonwillison.net](https://simonwillison.net/2024/Sep/20/using-llms-for-code/)

BrassCoders cites Willison's documented worries about AI-assisted coding — specifically the security review gap when developers trust AI output without reading it — as the practitioner framing of what the Perry et al. data shows empirically. Willison is explicit: the tool produces plausible-looking code; the developer who does not read it carefully will not catch the security issue; the issue ships. BrassCoders's pre-commit hook is one structural response to this gap.

- **What it's good for:** the practitioner-level security review gap in AI-assisted development.
- **Where BrassCoders draws from it:** the framing BrassCoders uses in product copy and the AI Blind Spots pillar to explain why the deterministic scanning layer sits underneath the assistant.

## FAQ

### What is vibe coding?

Vibe coding is Andrej Karpathy's term for fully AI-driven authorship. The developer describes what they want, the AI generates the code, and the developer vibe-checks the output — running it, seeing if it seems to work — without reading the code carefully. The name reflects the workflow: the developer is in the loop emotionally but not analytically. Karpathy defined it in a February 2025 post.

### Does vibe coding actually produce insecure code?

The empirical evidence says yes, and at a high rate. Perry et al. (ACM CCS 2023) found that developers using an AI assistant wrote significantly more insecure code than those without one — across injection, authorization, and cryptographic categories. Vibe coding compresses the review step Perry's participants still had. BrassCoders's own N=15 AI-code-findings corpus found real security issues in 9 of 15 AI-generated Python files, with zero generation-time warnings from the model.

### Why does the AI not warn about the security issue when it generates the code?

The model optimizes for code that satisfies the prompt, not code that is safe on attacker input. Unless the developer explicitly asks for a security review — and sometimes even when they do — the model generates the most statistically common completion. The most common pattern is often the insecure one, because insecure code is common in training data.

### What is the security review gap in AI-assisted coding?

Willison's framing: the AI produces plausible-looking code; the developer who does not read it carefully will not catch the security issue; the issue ships. The gap exists even in non-vibe-coding workflows, but vibe coding makes it structural — the point of vibe coding is to not read the code carefully. Perry et al.'s data shows developers using assistants were more likely to believe their code was secure, which is the overconfidence effect that makes the gap dangerous.

### How does BrassCoders fit into a vibe-coding workflow?

BrassCoders runs as a pre-commit hook or in CI. The scan takes 30 seconds on a typical codebase. If the AI-generated code has a hardcoded secret, a SQL injection pattern, a weak hash, or another detected issue, BrassCoders flags it before the commit lands — regardless of whether the developer read the code. The scan is the gate that vibe coding removes from the developer's workflow; BrassCoders puts it back in the pipeline.

---

*Generated from the canonical research config. CC BY 4.0. [Live version](https://coppersun.dev/research/vibe-coding-velocity-risk/) · [Install BrassCoders](https://coppersun.dev/install/).*
