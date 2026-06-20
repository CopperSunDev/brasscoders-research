# Will My AI-Generated Code Leak My Credentials?

> Secret leakage and credential exposure in AI-assisted development — the canonical research on how AI tools introduce and propagate secrets, and the tools that catch them before they ship.

BrassCoders treats secret leakage as the highest-frequency AI-coder bug category — not the most dramatic, but the most consistently present. AI coding assistants generate hardcoded credentials in configuration stubs, test scaffolding, and example code; LLMs memorize and reproduce credential-shaped strings from training data; developers share sensitive code with AI interfaces that transmit it to third-party servers.

[← Back to the index](../README.md) · [Live version](https://coppersun.dev/research/secret-leakage/)

## Sources (4)

---

### 📊 GitGuardian — State of Secrets Sprawl 2024
*GitGuardian, 2024* · [gitguardian.com/state-of-secrets-sprawl](https://www.gitguardian.com/state-of-secrets-sprawl)

BrassCoders treats GitGuardian's annual State of Secrets Sprawl report as the canonical industry-wide measurement of credential leakage in public repositories. The 2024 edition found over 12.8 million secrets exposed in public GitHub commits in a single year — an increase from prior editions — based on GitGuardian's real-time scanning of the public GitHub event stream. Builders making the case for credential scanning to a security team or compliance auditor should anchor on these numbers.

- **What it's good for:** primary-source citation for the scale of secret leakage, broken down by credential type and industry.
- **Where BrassCoders draws from it:** the lead evidence for secret leakage being a pervasive operational risk; referenced in the AI Blind Spots pillar under credential detection.

---

### 🏢 Samsung / ChatGPT Credential Disclosure (2023)
*Samsung Electronics, reported March 2023* · [TechCrunch coverage, May 2023](https://techcrunch.com/2023/05/02/samsung-bans-chatgpt-and-other-ai-tools-amid-employee-leaks/)

BrassCoders treats the Samsung incident as the canonical real-world demonstration that AI assistant interfaces are a credential transmission vector, not just a code generation tool. In March 2023, Samsung engineers pasted proprietary source code, internal meeting notes, and a database schema into ChatGPT during code review. Samsung confirmed the incident and implemented an internal ban on AI assistant use. The data sent to OpenAI's servers included what would qualify as trade secrets and internal configuration.

- **What it's good for:** demonstrating to non-technical stakeholders that AI assistant adoption has a concrete credential and IP transmission risk.
- **Where BrassCoders draws from it:** referenced when explaining why BrassCoders operates entirely locally by default — no code leaves the developer's machine during a free-tier scan.

---

### 📄 Carlini et al. — Quantifying Memorization Across Neural Language Models
*Nicholas Carlini, Daphne Ippolito, Matthew Jagielski, Katherine Lee, Florian Tramèr, Chiyuan Zhang · ICLR 2023* · [arxiv.org/abs/2202.07646](https://arxiv.org/abs/2202.07646)

BrassCoders treats this as the canonical academic evidence that language models memorize and reproduce verbatim sequences from training data — including API keys, email addresses, and phone numbers from leaked GitHub repositories. The paper quantifies how memorization scales with model size and training repetitions: larger models memorize more, and sequences that appear repeatedly in training data are reproduced at higher rates.

- **What it's good for:** primary-source evidence for why AI-generated code containing credential-shaped strings cannot be assumed to be placeholder values.
- **Where BrassCoders draws from it:** the theoretical underpinning for why BrassCoders treats every high-entropy flagged string as a potential real credential rather than a likely false positive.

---

### 🔧 detect-secrets (Yelp)
*Yelp Engineering · Python · GitHub: Yelp/detect-secrets · actively maintained* · [github.com/Yelp/detect-secrets](https://github.com/Yelp/detect-secrets)

BrassCoders treats detect-secrets as the canonical open-source Python credential scanner and bundles it as one of the 12 detection engines in every scan. Yelp built detect-secrets to block credential leakage in their own development pipeline; the public release covers high-entropy string detection, keyword-based detection (password, secret, key in assignment context), and regex patterns for known credential formats (AWS, Stripe, GitHub, and others).

- **What it's good for:** standalone credential scanning in any Python or polyglot repository; pre-commit hook integration.
- **Where BrassCoders draws from it:** bundled directly — detect-secrets findings appear in `.brass/security_report.yaml` tagged with the originating scanner.
