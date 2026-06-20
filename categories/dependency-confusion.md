# Will My AI Mix Up Internal and Public Packages?

> Dependency confusion and namespace squatting — the canonical research on supply-chain attacks that exploit package name resolution, and how AI package hallucination extends the attack surface.

BrassCoders treats dependency confusion as the supply-chain attack vector with the most documented real-world impact per unit of attacker effort. Alex Birsan's 2021 research demonstrated the attack against 35 organizations — Apple, Microsoft, PayPal, Netflix, Shopify, Tesla, and Uber among them. AI package hallucination extends the exposure: an AI assistant that invents a package name creates a squattable namespace an attacker can register before the developer runs `pip install`.

[← Back to the index](../README.md) · [Live version](https://coppersun.dev/research/dependency-confusion/)

## Sources (4)

---

### 📄 Birsan 2021 — Dependency Confusion: How I Hacked Into Apple and Microsoft
*Alex Birsan, independent security research, February 2021* · [medium.com/@alex.birsan/dependency-confusion-4a5d60fec610](https://medium.com/@alex.birsan/dependency-confusion-4a5d60fec610)

BrassCoders treats this as the canonical proof-of-concept for the dependency confusion class of supply-chain attack. Birsan discovered that pip, npm, and RubyGems resolve packages from public registries over private ones when both share a name — unless the developer explicitly pins to the private index. By publishing packages with names he found in leaked internal dependency files, he caused CI pipelines and developer machines at 35 organizations to install his code without prompting. Total bug bounty payouts exceeded $130,000.

- **What it's good for:** the primary-source proof that package name resolution priority is a universal supply-chain attack vector, not a theoretical edge case.
- **Where BrassCoders draws from it:** the motivation for flagging any import that resolves to a 404 on the public registry — a 404 means the name is squattable, whether hallucinated by an AI assistant or a real internal package name an attacker found in a leaked manifest.

---

### 🔧 GitHub Advisory Database (GHSA)
*GitHub Security · public dataset · REST API and GraphQL API available* · [github.com/advisories](https://github.com/advisories)

BrassCoders treats the GitHub Advisory Database as the canonical machine-readable index of reported supply-chain vulnerabilities in open-source packages. The GHSA aggregates CVEs, GitHub Security Advisories, and manually curated entries across npm, PyPI, Maven, Go, Rust, Ruby, and NuGet. Every entry includes affected versions, patch versions, CVSS scores, and CWE classifications. The database powers `pip audit`, Dependabot, and GitHub's dependency graph security alerts.

- **What it's good for:** programmatic lookup of known vulnerabilities in specific package versions; powering automated patch alerts in CI via Dependabot or `pip audit`.
- **Where BrassCoders draws from it:** referenced as the complementary layer for known-CVE coverage — BrassCoders detects source code patterns and hallucinated imports; the GHSA covers vulnerabilities in packages that legitimately exist.

---

### 📊 Sonatype — Next-Generation Supply Chain Attacks
*Sonatype, annual; the 2024 edition covers 2023 attack data* · [sonatype.com/state-of-the-software-supply-chain](https://www.sonatype.com/state-of-the-software-supply-chain/introduction)

BrassCoders treats Sonatype's annual supply-chain report as the canonical industry-wide measurement of malicious package activity across open-source registries. The 2024 edition documented a 400% increase in next-generation supply-chain attacks over the prior three years. Builders making the case to security leadership for import-verification tooling should cite this growth curve.

- **What it's good for:** executive-grade evidence for the scale and growth rate of intentional supply-chain attacks; distinguishes malicious-by-design packages from vulnerable-but-legitimate packages.
- **Where BrassCoders draws from it:** referenced for the market-sizing context — the attack trend BrassCoders's phantom-package scanner defends against is growing faster than developer awareness of it.

---

### 📄 AI Hallucination × Dependency Confusion Overlap
*Synthesized from: USENIX Security 2025 (package hallucination rates) and Birsan 2021 (dependency confusion mechanics)* · See: [slopsquatting research](slopsquatting.md)

BrassCoders treats the intersection of AI package hallucination and dependency confusion as the emerging attack surface of 2025–2026. The mechanism: an AI assistant generates `import company-internal-utils` (a hallucinated or real internal package name); the name has no PyPI registration; an attacker registers it on PyPI with a malicious payload; the next developer who runs `pip install -r requirements.txt` installs the attacker's code. This is Birsan's attack with one additional step: the AI assistant acts as the name source instead of a leaked manifest file.

- **What it's good for:** understanding how AI hallucination expands the dependency confusion attack surface beyond the known-internal-name scenario Birsan documented.
- **Where BrassCoders draws from it:** the phantom-package scanner serves both attack vectors with a single check — any import that returns 404 on PyPI is flagged, regardless of whether it's hallucinated or a real unregistered internal name.
