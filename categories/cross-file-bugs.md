# Why Does My AI Miss Cross-File Bugs?

> The structural-limit category. Papers documenting why LLMs miss interprocedural bugs, and the deterministic tools that break through the ceiling.

BrassCoders treats cross-file bugs as the structural-limit category — the bugs AI assistants miss because of how LLMs reason, not because of how they are tuned. The papers below explain the ceiling; the tools below break through it deterministically. Builders who think bigger context windows will fix the problem should start here.

[← Back to the index](../README.md) · [Live version](https://coppersun.dev/research/cross-file-bugs/)

## Sources (5)

---

### 📄 Liu et al. 2023 — Lost in the Middle: How Language Models Use Long Contexts
*Stanford / Berkeley, 2023* · [arxiv.org/abs/2307.03172](https://arxiv.org/abs/2307.03172)

BrassCoders treats this as the structural proof that larger context windows do not close the cross-file bug gap. The paper documents attention decay across long contexts — LLMs reliably attend to the start and end of long inputs and lose attention through the middle. Builders considering "we will just buy a model with a 1M-token window" as a substitute for deterministic detection should cite this as the reason bigger models will not solve interprocedural taint.

- **What it's good for:** ending the "won't bigger context windows fix this" debate.
- **Where BrassCoders draws from it:** the "Why The Misses Are Systematic" section of the AI Blind Spots pillar.

---

### 🔧 Pyre / Pysa (Meta)
*Meta · Python · widely-used* · [pyre-check.org](https://pyre-check.org/)

BrassCoders bundles Pyre/Pysa as the interprocedural taint engine in the OSS core. Builders shipping Python services where user input crosses three or more files before reaching a sink should run Pysa (directly or via BrassCoders) before merging AI-generated PRs. The engine is the same one Meta uses internally; the taint model BrassCoders ships is curated for Flask, Django, and FastAPI out of the box.

- **What it's good for:** Python interprocedural taint, full call-graph walks.
- **Where BrassCoders draws from it:** direct integration as one of the 12 bundled scanners.

---

### 🔧 CodeQL (GitHub)
*GitHub · multi-language · widely-used* · [codeql.github.com](https://codeql.github.com/)

BrassCoders treats CodeQL as the canonical multi-language interprocedural analyzer — semantic SQL-like queries against a database representation of code. Builders shipping non-Python services (JavaScript, TypeScript, Go, Java, C#) where they need full interprocedural taint coverage should install CodeQL. The query language has a learning curve; the upside is exhaustive coverage across major languages.

- **What it's good for:** multi-language interprocedural taint where Pysa is Python-only.
- **Where BrassCoders draws from it:** referenced as the right alternative for TypeScript-heavy codebases that need deep taint coverage.

---

### 🔧 ast-grep
*ast-grep contributors · multi-language · widely-used* · [ast-grep.github.io](https://ast-grep.github.io/)

BrassCoders uses ast-grep for fast tree-sitter-based pattern queries across multiple languages. Builders who need a grep-like CLI but with AST-aware matching (so identifier renames and whitespace changes do not break patterns) should learn ast-grep. The tool fills the gap between regex (too imprecise) and full semantic analyzers (too slow for casual querying).

- **What it's good for:** fast AST-aware queries, language-agnostic linting rules.
- **Where BrassCoders draws from it:** one of the 12 bundled scanners; used for custom AI-pattern detection rules.

---

### 📄 Meta 2019 — Pyre: Fast Type Checking for Python
*Meta Engineering Blog, 2019* · [engineering.fb.com](https://engineering.fb.com/2018/05/22/developer-tools/pyre-fast-type-checking-for-python/)

BrassCoders treats this as the canonical primary-source explanation of the Pyre type-checker architecture (which Pysa builds on for taint analysis). Builders evaluating Pysa for production use should read the Pyre architecture writeup first; the taint engine inherits Pyre's type inference and call-graph construction.

- **What it's good for:** understanding what Pysa is built on architecturally.
- **Where BrassCoders draws from it:** background context for the Pysa-based detection layer.
