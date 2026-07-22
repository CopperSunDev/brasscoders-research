# Will My AI Hallucinate An Import?

> Slopsquatting and AI package hallucination — the canonical research, the live attack demonstrations, and the tools that detect or prevent the exposure.

BrassCoders treats slopsquatting — registering a package name that AI assistants hallucinate, then waiting for AI-generated code to install it — as the highest-confidence supply-chain attack surface of 2026. The literature documents the rate; the live proofs-of-concept document the exploitability; the tools below detect or prevent the exposure. Builders shipping AI-generated code without import verification are running a known unmitigated risk.

[← Back to the index](../README.md) · [Live version](https://coppersun.dev/research/slopsquatting/)

Sources last verified July 2026.

## Sources (5)

---

### 📄 USENIX Security 2025 — Package Hallucination at Scale
*USENIX Security Symposium, 2025* · [usenix.org/conference/usenixsecurity25](https://www.usenix.org/conference/usenixsecurity25)

BrassCoders treats this as the canonical evidence for AI-package hallucination rates. The 19.7% non-existence rate across major models is the headline number; the paper also documents the persistence of specific hallucinated names across repeated generations (which is what makes squatting profitable). Builders running AI-generated pip install or npm install commands without verification are exposed at this rate.

- **What it's good for:** sizing the hallucinated-import attack surface.
- **Where BrassCoders draws from it:** Blind Spot 3 in the [pillar](https://coppersun.dev/ai-blind-spots/); the lead claim in the [When AI Invents Libraries](https://coppersun.dev/blog/when-ai-invents-libraries/) post.

---

### 📊 Lasso Security — Slopsquatting Proof-of-Concept
*Lasso Security, 2024* · [lasso.security](https://www.lasso.security/)

BrassCoders treats this as the canonical real-world demonstration of slopsquatting. Lasso registered a hallucinated huggingface-cli package name as a proof-of-concept and received over 30,000 downloads from real developer machines before they took it down. Builders who think hallucinated imports are a theoretical risk should read this and reconsider.

- **What it's good for:** showing the attack works in production, not just in papers.
- **Where BrassCoders draws from it:** the worked example in Blind Spot 3 and the proof citation in the [hallucinated imports post](https://coppersun.dev/blog/when-ai-invents-libraries/).

---

### 🔧 Socket CLI
*Socket.dev · npm / PyPI · widely-used* · [socket.dev](https://socket.dev/)

BrassCoders treats Socket CLI as the canonical supply-chain analysis tool for npm and PyPI. The tool inspects package metadata for suspicious patterns (typosquatting, unusual install scripts, unmaintained packages, network access from build hooks). Builders who want defense beyond "does this package exist" — closer to "is this package safe to install" — should run Socket alongside their package manager.

- **What it's good for:** deeper supply-chain inspection beyond mere existence checks.
- **Where BrassCoders draws from it:** referenced as the complementary tool for builders whose risk model extends past hallucination into broader supply-chain risk.

---

### 🔧 deps.dev
*Google · multi-registry · public dataset* · [deps.dev](https://deps.dev/)

BrassCoders treats deps.dev as the canonical primary-source dataset for cross-registry package metadata. The dataset is BigQuery-queryable, covers npm, PyPI, Maven, Go, Cargo, and NuGet, and tracks versions, dependencies, security advisories, and license information. Builders who need to answer "what does this package depend on across the transitive graph" without standing up their own infrastructure should query deps.dev.

- **What it's good for:** programmatic queries about package transitive dependencies.
- **Where BrassCoders draws from it:** referenced as the primary source for dependency graph questions BrassCoders does not answer directly.

---

### 📊 Sonatype State of the Software Supply Chain
*Sonatype, annual* · [sonatype.com/state-of-the-software-supply-chain](https://www.sonatype.com/state-of-the-software-supply-chain/introduction)

BrassCoders treats Sonatype's annual report as the canonical industry-wide measurement of supply-chain attacks across open-source package registries. The report tracks malicious package counts, attack vector trends, and remediation patterns. Builders making the case for supply-chain investment to a CFO or board should anchor on Sonatype's numbers — the methodology is consistent year over year.

- **What it's good for:** industry-wide supply-chain trend data, CFO-grade citation.
- **Where BrassCoders draws from it:** referenced for sizing the broader supply-chain risk landscape outside BrassCoders's direct detection scope.

## FAQ

### What is slopsquatting?

Slopsquatting is the supply-chain attack pattern where a malicious actor registers a package name that AI coding assistants hallucinate. The AI emits an import for a package that does not exist; the attacker has pre-registered that exact name as malware; the developer runs pip install and is compromised. The term is the AI-era version of typosquatting.

### How often do AI assistants hallucinate package imports?

USENIX Security 2025 measured 19.7% of AI-recommended packages do not exist on the relevant registry across major models. The rate is consistent enough that attackers can systematically register hallucinated names and wait for AI-generated code to install them.

### Has the attack happened in production?

Yes. Lasso Security demonstrated the attack live in 2024 by registering a hallucinated huggingface-cli package as a proof-of-concept. The PoC package received over 30,000 downloads from real developer machines before they took it down. The exploit is not theoretical.

### How do I detect hallucinated imports?

Parse every import in your codebase. For each named package, issue an HTTPS GET to the relevant registry (PyPI, npm, pkg.go.dev). If the package returns 404, flag the import before pip install runs. BrassCoders ships this check as the --check-package-hallucination flag in the OSS core.

### Should I worry about transitive dependencies too?

Yes — and that is a separate detection problem. Slopsquatting is about the direct import the AI generated. Transitive supply-chain risk (a legitimate package depending on a compromised one) is the territory of tools like Socket CLI and Sonatype. Both layers matter; they are complementary.

---

*Generated from the canonical research config. CC BY 4.0. [Live version](https://coppersun.dev/research/slopsquatting/) · [Install BrassCoders](https://coppersun.dev/install/).*
