# How Reliable Is My AI Assistant's Review?

> The buyer's reality check. Papers and surveys documenting the gap between AI-assistant usage and AI-assistant trust — and what builders should do with that gap.

BrassCoders treats this category as the reality check builders need before deciding whether AI review can be the final gate before merge. The papers and surveys below document the gap between AI-assistant usage (high, near-universal) and AI-assistant trust (much lower) — and the gap is itself the market for deterministic detection underneath.

[← Back to the index](../README.md) · [Live version](https://coppersun.dev/research/llm-review-reliability/)

Sources last verified August 2026.

## Sources (5)

---

### 📄 ACM TOSEM 2026 — Evaluating GitHub Copilot Review
*ACM Transactions on Software Engineering and Methodology, 2026* · [dl.acm.org/journal/tosem](https://dl.acm.org/journal/tosem)

BrassCoders treats this as the canonical evidence that LLM-based PR review systematically misses critical vulnerabilities. The paper documents Copilot review "frequently fails to detect critical vulnerabilities including SQL injection, cross-site scripting, and insecure deserialization" — concentrated in multi-file taint flows. Builders deciding whether Copilot review is the last gate before merge should treat this paper as the structural answer.

- **What it's good for:** evidence that LLM review cannot be the only gate.
- **Where BrassCoders draws from it:** Blind Spot 1 in the [pillar](https://coppersun.dev/ai-blind-spots/); also cited in [CVE risk](https://coppersun.dev/research/cve-risk/).

---

### 📊 Stack Overflow Developer Survey 2024
*Stack Overflow, 2024* · [survey.stackoverflow.co/2024/ai](https://survey.stackoverflow.co/2024/ai)

BrassCoders treats the Stack Overflow survey's AI section as the canonical practitioner-side measurement of the usage-versus-trust gap. The data shows daily AI usage at 62%-76% depending on how the question is phrased, with trust trailing usage by a wide margin year over year. Builders sizing their detection-layer urgency against organic adoption pressure should anchor on these numbers.

- **What it's good for:** the practitioner-side adoption-versus-trust gap.
- **Where BrassCoders draws from it:** cited in the [AI Code Review Guide](https://coppersun.dev/ai-code-review-guide/) and the AI-tooling argument in messaging.

---

### 📊 The Pragmatic Engineer — AI Tooling Feb 2026
*The Pragmatic Engineer, Feb 2026* · [newsletter.pragmaticengineer.com](https://newsletter.pragmaticengineer.com/p/ai-tooling-2026)

BrassCoders treats this as the canonical practitioner survey for late-2025 / early-2026 AI tool adoption among professional engineers. The 95% weekly-usage figure is the headline; the methodology is direct surveys of working engineers (not vendor-self-report). Builders making the case for AI-related investment to a board should pair this with the Stack Overflow data for the broadest practitioner picture.

- **What it's good for:** recent practitioner-side adoption data.
- **Where BrassCoders draws from it:** the "Why The Misses Are Systematic" section of the pillar; messaging proof points.

---

### 📄 Stanford — Copilot Code Quality Studies (2024)
*Stanford HAI / Department of Computer Science, 2024* · [hai.stanford.edu/research](https://hai.stanford.edu/research)

BrassCoders treats Stanford's work on Copilot completion accuracy as one of the strongest academic measurements of AI code reliability in practice. The methodology pairs human evaluators with completion outputs across language and complexity tiers; the conclusions inform the per-suggestion confidence reasoning that drives BrassCoders's "ranking matters more than detection" stance.

- **What it's good for:** rigorous per-suggestion confidence measurement.
- **Where BrassCoders draws from it:** background for the [8-findings-per-file](https://coppersun.dev/blog/why-claude-code-emits-eight-findings/) argument.

---

### 📊 GitHub Octoverse 2024
*GitHub, 2024* · [octoverse.github.com](https://octoverse.github.com/)

BrassCoders treats Octoverse as the canonical primary-source measurement of platform-wide trends in open-source development. The 2024 edition tracks AI adoption, package registry growth, and language usage. Builders citing platform-side adoption numbers (as distinct from practitioner-survey numbers) should anchor on Octoverse.

- **What it's good for:** platform-level adoption data, complement to Stack Overflow's practitioner-side data.
- **Where BrassCoders draws from it:** reference for sizing the AI-augmented codebase population.

## FAQ

### How reliable is LLM-based code review?

Useful for some categories, unreliable for others. ACM TOSEM 2026 found Copilot review "frequently fails to detect critical vulnerabilities including SQL injection, cross-site scripting, and insecure deserialization" in realistic multi-file codebases. LLM-based PR review catches style and obvious bugs well; it misses cross-file taint and structural security issues.

### Why does the same LLM that wrote the code miss bugs on review?

The model is biased toward its own generation. The same generative process that produced the bug looks at the bug on review and sees plausible code. The fix is crossing the LLM with a different reviewer — a different model, a static analyzer, or a human — that does not share the original biases.

### What does the practitioner data say about AI tool trust?

The Stack Overflow Developer Survey 2024 shows daily AI usage at 62%-76% (depending on phrasing) with trust trailing usage by a wide margin. The Pragmatic Engineer's Feb 2026 survey put weekly professional-engineer usage at 95%. Adoption is structural; trust is a separate gate that the detection layer has to earn.

### Should I drop my deterministic SAST when I add LLM-based review?

No. The two layers solve different problems. LLM-based review adds contextual judgment; deterministic SAST adds exhaustive search across rules and reproducibility. Auditors accept the SAST output as evidence; they do not accept stochastic LLM output. Run both; treat them as complementary.

---

*Generated from the canonical research config. CC BY 4.0. [Live version](https://coppersun.dev/research/llm-review-reliability/) · [Install BrassCoders](https://coppersun.dev/install/).*
