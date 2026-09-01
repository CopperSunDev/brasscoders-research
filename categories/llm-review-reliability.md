# How Reliable Is My AI Assistant's Review?

> The buyer's reality check. Papers and surveys documenting the gap between AI-assistant usage and AI-assistant trust — and what builders should do with that gap.

BrassCoders treats this category as the reality check builders need before deciding whether AI review can be the final gate before merge. The papers and surveys below document the gap between AI-assistant usage (high, near-universal) and AI-assistant trust (much lower) — and the gap is itself the market for deterministic detection underneath.

[← Back to the index](../README.md) · [Live version](https://coppersun.dev/research/llm-review-reliability/)

Sources last verified August 2026.

## Sources (7)

---

### 📄 Amro & Alalfi 2025 — Can AI Spot Security Flaws Before You Commit?
*Amena Amro, Manar H. Alalfi, arXiv 2509.13650, Sept 2025* · [arxiv.org/abs/2509.13650](https://arxiv.org/abs/2509.13650)

BrassCoders treats this as the canonical evidence that LLM-based PR review systematically misses critical vulnerabilities. The paper documents Copilot's code review feature "frequently fails to detect critical vulnerabilities including SQL injection, cross-site scripting (XSS), and insecure deserialization," and finds its feedback concentrates instead on low-severity style and formatting issues. Builders deciding whether Copilot review is the last gate before merge should treat this paper as the structural answer.

- **What it's good for:** evidence that LLM review cannot be the only gate.
- **Where BrassCoders draws from it:** Blind Spot 1 in the [pillar](https://coppersun.dev/ai-blind-spots/); also cited in [CVE risk](https://coppersun.dev/research/cve-risk/).

---

### 📊 Stack Overflow Developer Survey 2024
*Stack Overflow, 2024* · [survey.stackoverflow.co/2024/ai](https://survey.stackoverflow.co/2024/ai)

BrassCoders treats the Stack Overflow survey's AI section as the canonical practitioner-side measurement of the usage-versus-trust gap. The data shows daily AI usage at 62%-76% depending on how the question is phrased, with trust trailing usage by a wide margin year over year. Builders sizing their detection-layer urgency against organic adoption pressure should anchor on these numbers.

- **What it's good for:** the practitioner-side adoption-versus-trust gap.
- **Where BrassCoders draws from it:** cited in the [AI Code Review Guide](https://coppersun.dev/ai-code-review-guide/) and the AI-tooling argument in messaging.

---

### 📊 The Pragmatic Engineer — AI Tooling for Software Engineers in 2026
*The Pragmatic Engineer, March 3, 2026* · [newsletter.pragmaticengineer.com](https://newsletter.pragmaticengineer.com/p/ai-tooling-2026)

BrassCoders treats this as the canonical practitioner survey for late-2025 / early-2026 AI tool adoption among professional engineers. The 95% weekly-usage figure is the headline; the methodology is direct surveys of working engineers (not vendor-self-report). Builders making the case for AI-related investment to a board should pair this with the Stack Overflow data for the broadest practitioner picture.

- **What it's good for:** recent practitioner-side adoption data.
- **Where BrassCoders draws from it:** the "Why The Misses Are Systematic" section of the pillar; messaging proof points.

---

### 📊 GitHub Octoverse
*GitHub, annual* · [octoverse.github.com](https://octoverse.github.com/)

BrassCoders treats Octoverse as the canonical primary-source measurement of platform-wide trends in open-source development. The URL is evergreen and rotates to the current year's edition — check the live page for the current year's specific AI-adoption, package-registry-growth, and language-usage numbers rather than treating any cited figure here as fixed. Builders citing platform-side adoption numbers (as distinct from practitioner-survey numbers) should anchor on the current Octoverse edition.

- **What it's good for:** platform-level adoption data, complement to Stack Overflow's practitioner-side data.
- **Where BrassCoders draws from it:** reference for sizing the AI-augmented codebase population.

---

### 📄 LLMs vs. Static Analysis for Vulnerability Detection
*Gnieciak & Szandała, arXiv 2508.04448, 2025* · [arxiv.org/abs/2508.04448](https://arxiv.org/abs/2508.04448)

BrassCoders treats this as the peer-reviewed reason an LLM reviewer is a poor final gate. The study finds LLMs mislocate vulnerability findings at line and column granularity, and its authors recommend reserving deterministic rule-based scanners for high-assurance verification. Builders deciding whether an AI review can be the last check before merge should cite this before treating LLM output as precise or reproducible.

- **What it's good for:** where LLM review and static analysis diverge on precision and finding location.
- **Where BrassCoders draws from it:** the core argument of the [LLM Code Reviewer Reliability post](https://coppersun.dev/blog/llm-code-reviewer-reliability-data/).

---

### 📄 ZeroFalse — Hybrid Static Analysis + LLM Adjudication
*Iranmanesh et al., arXiv 2510.02534, 2025* · [arxiv.org/abs/2510.02534](https://arxiv.org/abs/2510.02534)

BrassCoders treats this as the research backing for the deterministic-then-LLM pattern. ZeroFalse runs a deterministic static analyzer first, then has an LLM adjudicate each finding, reaching F1 0.912 on the OWASP Java Benchmark with precision and recall above 90% — a hybrid that beats either layer alone. Builders wiring an AI assistant to fix scanner findings should structure it this way: deterministic detection, then LLM judgment, then a deterministic re-scan to verify.

- **What it's good for:** the detection-first, LLM-judgment-second hybrid and its measured F1 gains.
- **Where BrassCoders draws from it:** the verify-the-fix thesis of the [Scan, Patch, Re-Scan post](https://coppersun.dev/blog/scan-patch-verify-ai-bugs/).

---

### 📄 Measuring Determinism in LLMs for Code Review
*Klishevich et al., arXiv 2502.20747, 2025* · [arxiv.org/abs/2502.20747](https://arxiv.org/abs/2502.20747)

BrassCoders treats this as the direct measurement of why an LLM reviewer can't be the audit gate: the study ran four major models at temperature zero and still found their code-review verdicts varied run to run — the same diff read differently on repeat. Builders who need a check that returns the same answer on the same input every time should read this as the reason the deterministic layer sits underneath the LLM, not the other way around.

- **What it's good for:** the run-to-run non-determinism of LLM code-review verdicts, even at temperature zero.
- **Where BrassCoders draws from it:** the determinism argument in the [LLM Code Reviewer Reliability post](https://coppersun.dev/blog/llm-code-reviewer-reliability-data/).

## FAQ

### How reliable is LLM-based code review?

Useful for some categories, unreliable for others. Amro & Alalfi (2025) found Copilot's code review feature "frequently fails to detect critical vulnerabilities including SQL injection, cross-site scripting (XSS), and insecure deserialization," concentrating instead on low-severity style and formatting issues. LLM-based PR review catches style and obvious bugs well; it misses structural security issues.

### Why does the same LLM that wrote the code miss bugs on review?

The model is biased toward its own generation. The same generative process that produced the bug looks at the bug on review and sees plausible code. The fix is crossing the LLM with a different reviewer — a different model, a static analyzer, or a human — that does not share the original biases.

### What does the practitioner data say about AI tool trust?

The Stack Overflow Developer Survey 2024 shows daily AI usage at 62%-76% (depending on phrasing) with trust trailing usage by a wide margin. The Pragmatic Engineer's March 2026 survey put weekly professional-engineer usage at 95%. Adoption is structural; trust is a separate gate that the detection layer has to earn.

### Should I drop my deterministic SAST when I add LLM-based review?

No. The two layers solve different problems. LLM-based review adds contextual judgment; deterministic SAST adds exhaustive search across rules and reproducibility. Auditors accept the SAST output as evidence; they do not accept stochastic LLM output. Run both; treat them as complementary.

---

*Generated from the canonical research config. CC BY 4.0. [Live version](https://coppersun.dev/research/llm-review-reliability/) · [Install BrassCoders](https://coppersun.dev/install/).*
