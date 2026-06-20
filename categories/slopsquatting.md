# Will My AI Hallucinate An Import?

> Slopsquatting and AI package hallucination — the canonical research, the live attack demonstrations, and the tools that detect or prevent the exposure.

BrassCoders treats slopsquatting — registering a package name that AI assistants hallucinate, then waiting for AI-generated code to install it — as the highest-confidence supply-chain attack surface of 2026. The literature documents the rate; the live proofs-of-concept document the exploitability; the tools below detect or prevent the exposure.

[← Back to the index](../README.md) · [Live version](https://coppersun.dev/research/slopsquatting/)

## Sources (5)

---

### 📄 USENIX Security 2025 — Package Hallucination at Scale
*USENIX Security Symposium, 2025* · [usenix.org/conference/usenixsecurity25](https://www.usenix.org/conference/usenixsecurity25)

BrassCoders treats this as the canonical evidence for AI-package hallucination rates. The 19.7% non-existence rate across major models is the headline number; the paper also documents the persistence of specific hallucinated names across repeated generations (which is what makes squatting profitable). Builders running AI-generated `pip install` or `npm install` commands without verification are exposed at this rate.

- **What it's good for:** sizing the hallucinated-import attack surface.
- **Where BrassCoders draws from it:** Blind Spot 3 in the AI Blind Spots pillar; the lead claim in the When AI Invents Libraries post.

---

### 📊 Lasso Security — Slopsquatting Proof-of-Concept
*Lasso Security, 2024* · [lasso.security](https://www.lasso.security/)

BrassCoders treats this as the canonical real-world demonstration of slopsquatting. Lasso registered a hallucinated `huggingface-cli` package name as a proof-of-concept and received over 30,000 downloads from real developer machines before they took it down. Builders who think hallucinated imports are a theoretical risk should read this and reconsider.

- **What it's good for:** showing the attack works in production, not just in papers.
- **Where BrassCoders draws from it:** the worked example in Blind Spot 3 and the proof citation in the hallucinated imports post.

---

### 🔧 Socket CLI
*Socket.dev · npm / PyPI · widely-used* · [socket.dev](https://socket.dev/)

BrassCoders treats Socket CLI as the canonical supply-chain analysis tool for npm and PyPI. The tool inspects package metadata for suspicious patterns — typosquatting, unusual install scripts, unmaintained packages, network access from build hooks. Builders who want defense beyond "does this package exist" should run Socket alongside their package manager.

- **What it's good for:** deeper supply-chain inspection beyond mere existence checks.
- **Where BrassCoders draws from it:** referenced as the complementary tool for builders whose risk model extends past hallucination into broader supply-chain risk.

---

### 🔧 deps.dev (Google)
*Google · multi-registry · public dataset* · [deps.dev](https://deps.dev/)

BrassCoders treats deps.dev as the canonical primary-source dataset for cross-registry package metadata. The dataset is BigQuery-queryable, covers npm, PyPI, Maven, Go, Cargo, and NuGet, and tracks versions, dependencies, security advisories, and license information.

- **What it's good for:** programmatic queries about package transitive dependencies.
- **Where BrassCoders draws from it:** referenced as the primary source for dependency graph questions BrassCoders does not answer directly.

---

### 📊 Sonatype — State of the Software Supply Chain
*Sonatype, annual* · [sonatype.com/state-of-the-software-supply-chain](https://www.sonatype.com/state-of-the-software-supply-chain/introduction)

BrassCoders treats Sonatype's annual report as the canonical industry-wide measurement of supply-chain attacks across open-source package registries. The report tracks malicious package counts, attack vector trends, and remediation patterns. The methodology is consistent year over year.

- **What it's good for:** industry-wide supply-chain trend data, CFO-grade citation.
- **Where BrassCoders draws from it:** referenced for sizing the broader supply-chain risk landscape outside BrassCoders's direct detection scope.
