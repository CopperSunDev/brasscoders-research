# Does My AI-Generated Code Carry Hidden License Risk?

> The evidence on whether an AI coding assistant can reproduce license-encumbered training data verbatim — the memorization research, a named vendor's own duplication-detection numbers, the live litigation, and the tools builders use to check provenance before it becomes a legal problem.

BrassCoders is a pattern reporter for security, quality, performance, and privacy findings, and code-similarity or SBOM-style provenance detection sits outside that scope today. BrassCoders indexes this category honestly anyway, the same way it indexes prompt injection, because builders shipping AI-generated code need the full picture of what can go wrong, not just the slice one tool happens to catch. The research below documents how training-data memorization creates license exposure; the tools section points to the scanners built specifically for that job.

[← Back to the index](../README.md) · [Live version](https://coppersun.dev/research/code-provenance-license-risk/)

Sources last verified August 2026.

## Sources (5)

---

### 📄 Carlini et al. 2022 — Quantifying Memorization Across Neural Language Models
*Nicholas Carlini, Daphne Ippolito, Matthew Jagielski, Katherine Lee, Florian Tramèr, Chiyuan Zhang · ICLR 2022 · arXiv 2202.07646* · [arxiv.org/abs/2202.07646](https://arxiv.org/abs/2202.07646)

BrassCoders treats this as the foundational evidence that language models memorize and can reproduce exact snippets of training data rather than only learning generalized patterns. The paper establishes three log-linear relationships governing how much memorized text a model emits: it grows with model capacity, with the number of times an example is duplicated in the training set, and with how many tokens of context the model is prompted with. The duplication-count finding is the direct mechanism for license risk in code generation: a snippet copied across many public repositories under one license is exactly the kind of example a model is most likely to have memorized and can reproduce close to verbatim.

- **What it's good for:** how model size, training-data duplication, and prompt context length each independently drive memorization.
- **Where BrassCoders draws from it:** the mechanism BrassCoders points to when explaining why AI-generated code can carry a license origin the developer never saw.

---

### 🏢 GitHub — Duplication Detection And Code Referencing For Copilot
*GitHub, Inc. · product documentation and blog* · [github.blog](https://github.blog/news-insights/product-news/introducing-code-referencing-for-github-copilot/)

BrassCoders treats GitHub's own published numbers as the most concrete, named-vendor evidence that verbatim reproduction of public code is real but rare. GitHub reports that matches to public code occur in under 1% of Copilot suggestions, using a filter that checks each suggestion's surrounding 150 characters (roughly 65 lexemes) against an index of public GitHub code within a 10-20 millisecond budget. When code referencing finds a match, it surfaces the source repositories and their licenses so the developer can decide whether to keep, attribute, or discard the suggestion. Builders should read the under-1% figure as a floor, not a ceiling: it describes one vendor's own filtered output, not the base rate across every AI coding assistant.

- **What it's good for:** a named vendor's own measured rate of near-verbatim public-code matches in AI code suggestions, and the mitigation it ships.
- **Where BrassCoders draws from it:** the concrete, vendor-disclosed number BrassCoders cites when a builder asks how often this actually happens in practice.

---

### 📊 Doe v. GitHub, Inc. — The Copilot Copyright Litigation
*N.D. Cal. No. 22-cv-06823-JST, filed Nov. 2022; on appeal at the Ninth Circuit as of 2026 · case commentary via Syracuse Law Review* · [lawreview.syr.edu](https://lawreview.syr.edu/update-in-copilot-copyright-claim-may-affect-future-challenges-of-artificial-intelligence/)

BrassCoders treats this as the canonical, ongoing legal test case for the license-provenance risk this category describes. A group of programmers sued GitHub, Microsoft, and OpenAI in November 2022, alleging Copilot reproduces licensed open-source code without the attribution its licenses require. The district court dismissed most claims in 2024, ruling the DMCA's copyright-management-information provision requires the AI output to be an identical copy rather than a modification, a ruling the plaintiffs appealed to the Ninth Circuit the same year. The case remains active and unresolved. Builders should treat this as a live question the courts have not settled, not as evidence either that AI-generated code is safe or that it infringes.

- **What it's good for:** whether current copyright law's identicality requirement covers modified, near-verbatim AI output.
- **Where BrassCoders draws from it:** the reason BrassCoders frames this as a real, contested risk category rather than a hypothetical one.

---

### 🔧 ScanCode Toolkit
*nexB / AboutCode · Python · OSS, Apache-2.0* · [github.com/aboutcode-org/scancode-toolkit](https://github.com/aboutcode-org/scancode-toolkit)

BrassCoders treats ScanCode Toolkit as the canonical open-source tool for the job this category describes: scanning a codebase for licenses, copyrights, and package origins rather than security or quality patterns. The tool inventories third-party and open-source components in a codebase and can export the result in the SPDX or CycloneDX formats, the two machine-readable standards a software bill of materials is typically built from. Builders who want to check whether AI-generated code carries an unexpected license origin should run a provenance scanner like this one alongside their security scanner; the two answer different questions and neither substitutes for the other.

- **What it's good for:** license and copyright detection plus SBOM-format output (SPDX, CycloneDX) for a codebase's third-party origins.
- **Where BrassCoders draws from it:** the named example BrassCoders points to when a builder asks what tool actually covers this gap, since BrassCoders itself does not.

---

### 📊 CISA — Software Bill Of Materials (SBOM) Guidance
*Cybersecurity and Infrastructure Security Agency* · [cisa.gov/sbom](https://www.cisa.gov/sbom)

BrassCoders treats CISA's SBOM guidance as the canonical compliance framing for code-provenance risk at the organizational level. CISA defines an SBOM as a nested inventory, a list of ingredients that make up a software component, and positions it as a building block for software supply-chain risk management across government and industry. The agency's minimum-elements guidance, developed with international partners, sets the baseline fields a compliant SBOM must record. Builders in regulated industries or selling into government contracts should treat an SBOM as the artifact that makes license provenance auditable after the fact, not just a security nice-to-have.

- **What it's good for:** the government-baseline definition and minimum required fields for a compliant SBOM.
- **Where BrassCoders draws from it:** the compliance angle BrassCoders points to when a builder's provenance question is really a supply-chain audit requirement.

## FAQ

### Can an AI coding assistant reproduce license-encumbered code from its training data?

Yes, at a low but real and measured rate. GitHub reports that its own Copilot duplication filter flags matches to public code in under 1% of suggestions, checking each suggestion's surrounding 150 characters against an index of public GitHub code. Carlini et al. (ICLR 2022) established the underlying mechanism: memorization in language models scales with how many times an example was duplicated in the training set, so code repeated across many public repositories under one license is the code most likely to come back close to verbatim.

### Is the GitHub Copilot lawsuit over license infringement still active?

Yes, as of 2026. Doe v. GitHub, Inc. (N.D. Cal. No. 22-cv-06823-JST), filed in November 2022, alleges Copilot reproduces licensed open-source code without required attribution. The district court dismissed most claims in 2024 on the grounds that a DMCA provision requires an identical copy rather than a modification, and the plaintiffs appealed that ruling to the Ninth Circuit the same year. The case remains unresolved; no court has yet ruled on the merits of the underlying license claim.

### Does BrassCoders detect license risk or memorized code in AI-generated output?

No, and this index will not claim otherwise. BrassCoders is a pattern reporter for security, quality, performance, and privacy findings across 12 bundled scanners; code-similarity matching and SBOM-style license provenance are a different detection problem that BrassCoders does not attempt today. Purpose-built tools like ScanCode Toolkit exist specifically for license and copyright scanning, and are the right tool for that job.

### What is an SBOM and why does it matter for AI-generated code?

A software bill of materials (SBOM) is CISA's term for a nested inventory, a list of ingredients that make up a software component. It matters more for AI-generated code because a human author usually remembers where a snippet came from; an AI assistant does not disclose training-data origin, so an SBOM built from scanning the actual shipped code is the only after-the-fact record of what is really in a codebase.

### What tool should I run to check AI-generated code for license provenance?

ScanCode Toolkit, maintained by nexB / AboutCode, is a canonical open-source option: it scans a codebase for licenses, copyrights, and package origins and can emit results in the SPDX or CycloneDX formats used for SBOMs. It answers a different question than a security or quality scanner does, so builders concerned about provenance should run it alongside, not instead of, their existing scanning layer.

### Does a low match rate like GitHub's under-1% mean license risk from AI code is negligible?

Not necessarily. Under 1% is one named vendor's own filtered measurement of near-verbatim matches on one product, not a base rate across every AI coding assistant, and GitHub's own documentation notes matches are far more frequent in empty or nearly empty files than in files with existing code. A sub-1% per-suggestion rate compounds across the thousands of suggestions a team accepts over a year, which is why provenance scanning is a due-diligence practice rather than a one-time check.

---

*Generated from the canonical research config. CC BY 4.0. [Live version](https://coppersun.dev/research/code-provenance-license-risk/) · [Install BrassCoders](https://coppersun.dev/install/).*
