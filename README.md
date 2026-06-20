# BrassCoders Research Index

**The literature that informs BrassCoders's detection layer — papers, surveys, benchmarks, and tools organized by the question a builder actually asks.**

Live version: [coppersun.dev/research/](https://coppersun.dev/research/)

---

## What This Is

AI coding assistants ship bugs at a structural rate. BrassCoders is a static-analysis CLI that runs 12 scanners against AI-generated Python code and emits findings in a format AI assistants can consume. The research in this repository is the evidence base BrassCoders is built on — every claim in the product documentation and marketing has a primary-source citation in these files.

The index is organized around the questions developers actually ask when evaluating whether to add a detection layer to their AI-assisted workflow. Each category links to a live web version with the same content plus commentary.

---

## Eight Categories

| # | Question | Sources | Live page |
|---|---|---|---|
| 1 | [Will my AI-generated code ship a CVE?](categories/cve-risk.md) | 10 | [/research/cve-risk/](https://coppersun.dev/research/cve-risk/) |
| 2 | [Will my AI hallucinate an import?](categories/slopsquatting.md) | 5 | [/research/slopsquatting/](https://coppersun.dev/research/slopsquatting/) |
| 3 | [Why does my AI miss cross-file bugs?](categories/cross-file-bugs.md) | 5 | [/research/cross-file-bugs/](https://coppersun.dev/research/cross-file-bugs/) |
| 4 | [How reliable is my AI assistant's review?](categories/llm-review-reliability.md) | 5 | [/research/llm-review-reliability/](https://coppersun.dev/research/llm-review-reliability/) |
| 5 | [What does the AI-coding market look like in 2026?](categories/market-intelligence.md) | 5 | [/research/market-intelligence/](https://coppersun.dev/research/market-intelligence/) |
| 6 | [Will my AI write slow code?](categories/performance-anti-patterns.md) | 7 | [/research/performance-anti-patterns/](https://coppersun.dev/research/performance-anti-patterns/) |
| 7 | [Will my AI-generated code leak my credentials?](categories/secret-leakage.md) | 4 | [/research/secret-leakage/](https://coppersun.dev/research/secret-leakage/) |
| 8 | [Will my AI mix up internal and public packages?](categories/dependency-confusion.md) | 4 | [/research/dependency-confusion/](https://coppersun.dev/research/dependency-confusion/) |

Total: **45 sources** across 8 categories.

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

Schema:
```json
{
  "version": "1.0",
  "updated": "2026-06-20",
  "categories": [
    {
      "id": "cve-risk",
      "question": "Will my AI-generated code ship a CVE?",
      "url": "https://coppersun.dev/research/cve-risk/",
      "source_count": 10,
      "sources": [
        {
          "title": "...",
          "type": "paper|survey|company|tool|benchmark",
          "author": "...",
          "year": 2026,
          "url": "..."
        }
      ]
    }
  ]
}
```

---

## How This Index Is Built

- Sources are curated by the BrassCoders team based on primary-source quality, methodology transparency, and direct relevance to AI-generated code risks.
- Every claim in BrassCoders documentation that cites research is backed by a source in this index.
- Each source file documents not only what the source found, but **what BrassCoders draws from it** — so the connection between the research and the product claim is explicit and auditable.
- The index is updated each time a new research category is added to coppersun.dev/research/.

---

## License

This research index is published under [CC BY 4.0](LICENSE). You are free to share and adapt it for any purpose, including commercial, as long as you credit BrassCoders.

The underlying papers, reports, and tools are each subject to their own licenses. Links are provided to primary sources.

---

## Related

- **BrassCoders CLI** (Apache 2.0): [github.com/CopperSunDev/brasscoders](https://github.com/CopperSunDev/brasscoders)
- **BrassCoders product**: [coppersun.dev](https://coppersun.dev/)
- **Install**: `pip install brasscoders`
