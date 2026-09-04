# BrassCoders Research Index

**The literature that informs BrassCoders's detection layer — papers, surveys, benchmarks, and tools organized by the question a builder actually asks.**

Live version: [coppersun.dev/research/](https://coppersun.dev/research/)

Sources last verified August 2026.

---

## What This Is

AI coding assistants ship bugs at a structural rate. BrassCoders is a static-analysis CLI that runs 12 scanners against AI-generated Python code and emits findings in a format AI assistants can consume. The research in this repository is the evidence base BrassCoders is built on — every claim in the product documentation and marketing has a primary-source citation in these files.

The index is organized around the questions developers actually ask when evaluating whether to add a detection layer to their AI-assisted workflow. Each category links to a live web version with the same content plus commentary.

> This repository is **generated from a single canonical config** and is never hand-edited. To propose a change, open an issue or edit the source index at coppersun.dev.

---

## 17 Categories

| # | Question | Sources | Live page |
|---|---|---|---|
| 1 | [Will My AI-Generated Code Ship A CVE?](categories/cve-risk.md) | 12 | [/research/cve-risk/](https://coppersun.dev/research/cve-risk/) |
| 2 | [Will My AI Hallucinate An Import?](categories/slopsquatting.md) | 6 | [/research/slopsquatting/](https://coppersun.dev/research/slopsquatting/) |
| 3 | [Why Does My AI Miss Cross-File Bugs?](categories/cross-file-bugs.md) | 5 | [/research/cross-file-bugs/](https://coppersun.dev/research/cross-file-bugs/) |
| 4 | [How Reliable Is My AI Assistant's Review?](categories/llm-review-reliability.md) | 7 | [/research/llm-review-reliability/](https://coppersun.dev/research/llm-review-reliability/) |
| 5 | [What Does The AI-Coding Market Look Like In 2026?](categories/market-intelligence.md) | 6 | [/research/market-intelligence/](https://coppersun.dev/research/market-intelligence/) |
| 6 | [Will My AI Write Slow Code?](categories/performance-anti-patterns.md) | 7 | [/research/performance-anti-patterns/](https://coppersun.dev/research/performance-anti-patterns/) |
| 7 | [Will My AI-Generated Code Leak My Credentials?](categories/secret-leakage.md) | 4 | [/research/secret-leakage/](https://coppersun.dev/research/secret-leakage/) |
| 8 | [Will My AI Mix Up Internal and Public Packages?](categories/dependency-confusion.md) | 4 | [/research/dependency-confusion/](https://coppersun.dev/research/dependency-confusion/) |
| 9 | [Will My AI Use Weak Crypto?](categories/weak-crypto.md) | 5 | [/research/weak-crypto/](https://coppersun.dev/research/weak-crypto/) |
| 10 | [Can My AI Coding Agent Be Hijacked?](categories/prompt-injection.md) | 5 | [/research/prompt-injection/](https://coppersun.dev/research/prompt-injection/) |
| 11 | [What Does Coding At AI Speed Do To Security And Quality?](categories/vibe-coding-velocity-risk.md) | 3 | [/research/vibe-coding-velocity-risk/](https://coppersun.dev/research/vibe-coding-velocity-risk/) |
| 12 | [How Do I Secure Code That AI Agents Write Autonomously, Or Code That Uses Agents?](categories/agentic-ai-code-security.md) | 4 | [/research/agentic-ai-code-security/](https://coppersun.dev/research/agentic-ai-code-security/) |
| 13 | [What Do HIPAA, GDPR, SOC 2, And Other Regulations Require When AI Writes Your Code?](categories/ai-code-regulated-industries.md) | 4 | [/research/ai-code-regulated-industries/](https://coppersun.dev/research/ai-code-regulated-industries/) |
| 14 | [Will My AI-Generated API Let One User See Another User's Data?](categories/access-control-idor.md) | 5 | [/research/access-control-idor/](https://coppersun.dev/research/access-control-idor/) |
| 15 | [Will My AI Use pickle.load Or yaml.load On Untrusted Data?](categories/insecure-deserialization.md) | 8 | [/research/insecure-deserialization/](https://coppersun.dev/research/insecure-deserialization/) |
| 16 | [Does My AI-Generated Code Carry Hidden License Risk?](categories/code-provenance-license-risk.md) | 5 | [/research/code-provenance-license-risk/](https://coppersun.dev/research/code-provenance-license-risk/) |
| 17 | [Will My AI Write A Regex That Hangs My Server?](categories/redos-resource-exhaustion.md) | 7 | [/research/redos-resource-exhaustion/](https://coppersun.dev/research/redos-resource-exhaustion/) |

Total: **97 sources** across 17 categories.

---

## Source Types

Each source in the category files is tagged by type:

| Tag | Meaning |
|---|---|
| 📄 | Academic paper or peer-reviewed research |
| 📊 | Industry survey, analyst report, or market data |
| 🏢 | Primary-source company disclosure (SEC filing, earnings, investor comms) |
| 🔧 | Open-source tool or library (directly usable) |
| 🧪 | Benchmark corpus or intentionally-vulnerable test project |

---

## Machine-Readable Data

The full index in machine-readable form: [`data/research.json`](data/research.json)

Each entry carries `slug`, `type` (paper/report/tool/benchmark/disclosure), `title`, `source` (byline), and `url`. Categories carry `id`, `question`, live `url`, and `source_count`. An AI-readable summary lives at [`llms.txt`](llms.txt).

---

## License

Content licensed [CC BY 4.0](LICENSE) — free to use and adapt with attribution to BrassCoders (coppersun.dev). The annotations are BrassCoders's applied commentary; the underlying sources belong to their respective authors.
