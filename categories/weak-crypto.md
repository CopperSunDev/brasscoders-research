# Will My AI Use Weak Crypto?

> The canonical evidence on weak cryptography in AI-generated code — the papers measuring the rate, the corpus for testing detection, and the deterministic rules that flag it.

BrassCoders treats weak cryptography as one of the highest-confidence AI-code failure modes, because the model learned it from a decade of tutorials that reach for MD5, SHA1, and AES-ECB. The literature measures the rate; the corpora provide ground truth; the deterministic rules below flag every instance the same way on every run. Builders who assume a frontier model defaults to safe crypto primitives should read the numbers first.

[← Back to the index](../README.md) · [Live version](https://coppersun.dev/research/weak-crypto/)

Sources last verified July 2026.

## Sources (5)

---

### 📄 Pearce et al. 2022 — Asleep at the Keyboard? Assessing the Security of GitHub Copilot's Code Contributions
*IEEE Symposium on Security and Privacy, 2022* · [arxiv.org/abs/2108.09293](https://arxiv.org/abs/2108.09293)

BrassCoders treats this as the canonical measurement of how often AI-generated code ships insecure patterns, weak cryptography among them. Across 89 scenarios drawn from MITRE's CWE Top 25, the authors generated 1,689 programs with GitHub Copilot and found approximately 40% vulnerable — with the cryptography scenarios, CWE-327 (use of a broken or risky cryptographic algorithm), among the categories the model handled worst. Builders who assume the assistant defaults to safe primitives should treat this as the reason to add a deterministic check underneath.

- **What it's good for:** sizing the rate at which AI assistants emit broken-crypto and other CWE-Top-25 patterns.
- **Where BrassCoders draws from it:** the opening evidence for the weak-crypto detection layer in the [AI Blind Spots pillar](https://coppersun.dev/ai-blind-spots/).

---

### 📄 Perry et al. 2023 — Do Users Write More Insecure Code with AI Assistants?
*ACM CCS 2023 (Perry, Srivastava, Kumar, Boneh)* · [arxiv.org/abs/2211.03622](https://arxiv.org/abs/2211.03622)

BrassCoders treats this as the canonical human-factors evidence on AI-assisted crypto. In a controlled user study spanning security-sensitive tasks — including one asking participants to encrypt and sign a message — those with access to an AI assistant wrote significantly less secure code, and were more likely to believe their code was secure than the control group. The confidence gap is the dangerous part: developers trust weak crypto more when an assistant produced it. Builders relying on developer review to catch crypto misuse should treat this as the reason review alone is not enough.

- **What it's good for:** the overconfidence effect that lets AI-suggested weak crypto pass human review.
- **Where BrassCoders draws from it:** the "deterministic gate, not just review" argument threaded through the [security-gate post](https://coppersun.dev/blog/llm-reviewer-not-a-security-gate/).

---

### 📄 Fu et al. 2025 — Security Weaknesses of Copilot-Generated Code in GitHub Projects
*ACM TOSEM 2025 (Fu, Liang, Tahir, Li, Shahin, Yu, Chen)* · [arxiv.org/abs/2310.02059](https://arxiv.org/abs/2310.02059)

BrassCoders treats this as the canonical field measurement of AI crypto weakness in real repositories. Studying Copilot-generated snippets merged into public GitHub projects, the authors found security weaknesses in 29.5% of Python and 24.2% of JavaScript snippets, with CWE-330 (use of insufficiently random values) the single most frequent weakness — the randomness failure that undermines tokens, nonces, and keys. They also report that feeding static-analysis warnings back through the assistant fixed up to 55.5% of the issues. Builders get the number that justifies a deterministic scanner in the loop.

- **What it's good for:** proving weak randomness is the most frequent crypto failure in shipped AI code, and that static analysis measurably fixes it.
- **Where BrassCoders draws from it:** the deterministic-scanner-plus-AI loop BrassCoders is built around.

---

### 🧪 Tihanyi et al. 2023 — The FormAI Dataset: Generative AI in Software Security Through the Lens of Formal Verification
*PROMISE / arXiv, 2023* · [arxiv.org/abs/2307.02192](https://arxiv.org/abs/2307.02192)

BrassCoders treats FormAI as the largest formally-verified corpus of AI-generated vulnerable code. The authors generated 112,000 C programs with GPT-3.5 and labeled vulnerabilities with the ESBMC bounded model checker; 51.24% of the programs contained at least one vulnerability, classified with CWE numbers that include broken and risky cryptographic usage. Because the labeling is formal verification rather than a heuristic linter, the rate is a floor, not an estimate. Builders who want a ground-truth corpus for testing crypto and memory-safety detection should start here.

- **What it's good for:** a formally-verified ground-truth corpus for benchmarking crypto and memory-safety detection.
- **Where BrassCoders draws from it:** reference corpus shape for how BrassCoders validates deterministic detection against known-vulnerable code; complements the [BrassCoders benchmarks](https://coppersun.dev/benchmarks/).

---

### 🔧 Bandit — Cryptography and Randomness Rules (B303, B304/B305, B324, B311)
*PyCQA · Python · widely-used* · [bandit.readthedocs.io](https://bandit.readthedocs.io/en/latest/plugins/index.html)

BrassCoders bundles Bandit's cryptography rule set as its deterministic weak-crypto detector. The rules fire on the exact patterns AI assistants over-produce: B303 for MD5 and SHA1, B304 and B305 for insecure ciphers and cipher modes such as DES and AES-ECB, B324 for weak hash algorithms passed to hashlib, and B311 for the standard random module used where a cryptographically secure generator is required. Every match is a structural pattern, reported the same way on every run. Builders who want one command that flags all of these — plus eleven other scanners — should run BrassCoders; builders who want only Python crypto linting can run Bandit directly.

- **What it's good for:** deterministic detection of MD5/SHA1, ECB and weak ciphers, weak hashlib calls, and insecure random.
- **Where BrassCoders draws from it:** one of the 12 bundled scanners; the crypto layer behind the [Weak Crypto in AI-Generated Python](https://coppersun.dev/blog/weak-crypto-ai-generated-python/) post.

## FAQ

### How often does AI-generated code use weak cryptography?

Multiple studies converge on a high rate. Pearce et al. (IEEE S&P 2022) found approximately 40% of 1,689 Copilot-generated programs vulnerable across CWE Top 25, with the CWE-327 cryptography scenarios among the worst. Fu et al. (ACM TOSEM 2025) found CWE-330 (insufficiently random values) the single most frequent weakness in real Copilot snippets. FormAI's formal-verification labeling put the overall vulnerability rate at 51.24% across 112,000 generated C programs.

### What weak-crypto patterns do AI assistants generate most?

MD5 and SHA1 for password hashing, AES in ECB mode, DES and other broken ciphers, hardcoded initialization vectors, and — most frequently in the field data — the standard random module used to generate tokens, nonces, and keys where a cryptographically secure generator is required. Fu et al. found this last pattern, CWE-330, the most common weakness of all in shipped Copilot code.

### Why do AI assistants pick weak crypto?

Training data. A decade of tutorials, Stack Overflow answers, and sample code reach for md5() and AES-ECB because they are short and they run. The model completes the prompt with the most common pattern it saw, and the most common pattern is often the insecure one. The security difference only appears against an attacker, which the prompt rarely describes.

### Can code review catch weak crypto?

Unreliably, especially when an AI assistant produced the code. Perry et al. (CCS 2023) found that developers using an AI assistant wrote less secure code on crypto tasks and were more likely to believe it was secure than developers without one. The overconfidence effect means weak crypto passes review more easily when it came from an assistant — which is why a deterministic scanner belongs underneath the review.

### What tools detect weak crypto in Python?

Bandit's cryptography rules: B303 (MD5/SHA1), B304 and B305 (insecure ciphers and modes like DES and AES-ECB), B324 (weak hashlib algorithms), and B311 (insecure random for security contexts). BrassCoders bundles Bandit alongside eleven other scanners so these fire on every scan with unified output; running Bandit alone covers the Python crypto patterns specifically.

---

*Generated from the canonical research config. CC BY 4.0. [Live version](https://coppersun.dev/research/weak-crypto/) · [Install BrassCoders](https://coppersun.dev/install/).*
