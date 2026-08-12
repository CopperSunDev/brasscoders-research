# Will My AI-Generated Code Leak My Credentials?

> Secret leakage and credential exposure in AI-assisted development — the canonical research on how AI tools introduce and propagate secrets, and the tools that catch them before they ship.

BrassCoders treats secret leakage as the highest-frequency AI-coder bug category — not the most dramatic, but the most consistently present. AI coding assistants generate hardcoded credentials in configuration stubs, test scaffolding, and example code; LLMs memorize and reproduce credential-shaped strings from training data; developers share sensitive code with AI interfaces that transmit it to third-party servers. The literature on each mechanism is deep. The detection is mechanical.

[← Back to the index](../README.md) · [Live version](https://coppersun.dev/research/secret-leakage/)

Sources last verified August 2026.

## Sources (4)

---

### 📊 GitGuardian — State of Secrets Sprawl 2024
*GitGuardian, 2024* · [gitguardian.com/state-of-secrets-sprawl](https://www.gitguardian.com/state-of-secrets-sprawl)

BrassCoders treats GitGuardian's annual State of Secrets Sprawl report as the canonical industry-wide measurement of credential leakage in public repositories. The 2024 edition found over 12.8 million secrets exposed in public GitHub commits in a single year — an increase from prior editions — based on GitGuardian's real-time scanning of the public GitHub event stream. Builders making the case for credential scanning to a security team or compliance auditor should anchor on these numbers.

- **What it's good for:** primary-source citation for the scale of secret leakage in the wild, broken down by credential type (cloud keys, database URLs, API tokens) and industry.
- **Where BrassCoders draws from it:** the lead evidence for secret leakage being a pervasive operational risk rather than a hypothetical; referenced in the [AI Blind Spots pillar](https://coppersun.dev/ai-blind-spots/) under credential detection.

---

### 🏢 Samsung / ChatGPT Credential Disclosure (2023)
*Samsung Electronics, reported March 2023* · [TechCrunch coverage, May 2023](https://techcrunch.com/2023/05/02/samsung-bans-chatgpt-and-other-ai-tools-amid-employee-leaks/)

BrassCoders treats the Samsung incident as the canonical real-world demonstration that AI assistant interfaces are a credential transmission vector, not just a code generation tool. In March 2023, Samsung engineers pasted proprietary source code, internal meeting notes, and a database schema into ChatGPT during code review. Samsung confirmed the incident and implemented an internal ban on AI assistant use for a period. The data sent to OpenAI's servers included what would qualify as trade secrets and internal configuration.

- **What it's good for:** demonstrating to non-technical stakeholders (legal, compliance, executives) that AI assistant adoption has a concrete credential and IP transmission risk.
- **Where BrassCoders draws from it:** referenced when explaining why BrassCoders operates entirely locally by default — no code leaves the developer's machine during a free-tier scan.

---

### 📄 Carlini et al. — Quantifying Memorization Across Neural Language Models
*Nicholas Carlini, Daphne Ippolito, Matthew Jagielski, Katherine Lee, Florian Tramèr, Chiyuan Zhang · ICLR 2023* · [arxiv.org/abs/2202.07646](https://arxiv.org/abs/2202.07646)

BrassCoders treats this as the canonical academic evidence that language models memorize and reproduce verbatim sequences from training data — including API keys, email addresses, and phone numbers from leaked GitHub repositories. The paper quantifies how memorization scales with model size and training repetitions: larger models memorize more, and sequences that appear repeatedly in training data are reproduced at higher rates. The 2021 predecessor paper demonstrated the attack against GPT-2; this 2023 paper establishes the general law.

- **What it's good for:** primary-source evidence for why AI-generated code containing credential-shaped strings cannot be assumed to be placeholder values — the model may have reproduced a real credential from training data.
- **Where BrassCoders draws from it:** the theoretical underpinning for why BrassCoders treats every high-entropy flagged string as a potential real credential rather than a likely false positive.

---

### 🔧 detect-secrets (Yelp)
*Yelp Engineering · Python · GitHub: Yelp/detect-secrets · actively maintained* · [github.com/Yelp/detect-secrets](https://github.com/Yelp/detect-secrets)

BrassCoders treats detect-secrets as the canonical open-source Python credential scanner and bundles it as one of the 12 detection engines in every scan. Yelp built detect-secrets to block credential leakage in their own development pipeline; the public release covers high-entropy string detection, keyword-based detection (password, secret, key in assignment context), and regex patterns for known credential formats (AWS, Stripe, GitHub, and others). It operates as a pre-commit hook or CI tool independently; inside BrassCoders it contributes findings to the unified YAML output.

- **What it's good for:** standalone credential scanning in any Python or polyglot repository; pre-commit hook integration for blocking secrets before commit.
- **Where BrassCoders draws from it:** bundled directly — detect-secrets findings appear in `.brass/security_report.yaml` tagged with the originating scanner, so builders know which engine flagged each credential pattern.

## FAQ

### How do AI coding assistants cause secret leakage?

Three mechanisms. First, AI assistants trained on public code have memorized credential patterns from leaked repositories — and can reproduce them verbatim when context triggers recall. Second, AI-generated code often hardcodes credentials for convenience, especially in configuration stubs and test scaffolding. Third, developers paste proprietary code or credentials into AI assistant prompts, which sends the data to third-party servers. BrassCoders detects the second mechanism (hardcoded credentials in source) at scan time.

### What happened at Samsung with AI assistant credential leakage?

In March 2023, Samsung engineers used ChatGPT to assist with code review and debugging. In the process, they pasted proprietary source code and internal meeting notes — including a database schema — into the ChatGPT interface. Samsung confirmed the incident and temporarily banned AI assistant use internally. The incident demonstrated that data shared with AI assistants travels to third-party servers, and that credential patterns in code are particularly sensitive.

### Can LLMs actually reproduce credentials they were trained on?

Yes. Carlini et al. (2021 and 2023) demonstrated that language models memorize and can reproduce verbatim sequences from their training data, including email addresses, phone numbers, and API keys from leaked GitHub repositories. The 2023 paper quantified the memorization rate across model sizes and showed it increases with model scale. Builders should treat AI-generated code with embedded secret-shaped strings as a red flag, not a false positive.

### What does BrassCoders detect in this category?

BrassCoders bundles detect-secrets (Yelp's OSS credential scanner) plus a custom secret-pattern scanner. Together they flag: high-entropy strings in source code, known credential prefixes (AWS access key IDs starting with AKIA, GitHub personal access tokens starting with ghp_, Stripe keys, Anthropic keys, and others), private key PEM blocks, and .env-style assignments in source files. Every flagged credential should be treated as compromised and rotated — even if it looks like a placeholder.

### What is GitGuardian's State of Secrets Sprawl?

GitGuardian's annual report is the most cited primary-source measurement of credential leakage in public repositories. The 2024 edition found over 12.8 million secrets exposed in public GitHub commits in a single year. It tracks trends by credential type (cloud keys, database URLs, API tokens), industry, and repository age. The findings are based on GitGuardian's real-time scanning of the public GitHub event stream.

### How do I prevent AI tools from sending credentials to third-party servers?

Four controls. First, never paste credentials, database URLs, or proprietary configuration into an AI assistant chat interface. Second, use the AI assistant's VS Code extension or IDE integration rather than the web interface when working with sensitive code — some configurations offer local context modes. Third, add .env files and secrets managers to your .gitignore and never hardcode secrets in source. Fourth, run BrassCoders scan on every commit to catch hardcoded credentials before they reach a shared repository.

---

*Generated from the canonical research config. CC BY 4.0. [Live version](https://coppersun.dev/research/secret-leakage/) · [Install BrassCoders](https://coppersun.dev/install/).*
