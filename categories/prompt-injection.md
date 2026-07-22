# Can My AI Coding Agent Be Hijacked?

> The runtime attack surface. The canonical research on prompt injection against AI coding agents — and an honest map of where deterministic source scanning ends and agent-runtime mitigation begins.

BrassCoders scans source code deterministically, and prompt injection sits deliberately outside that scope: it is a runtime attack on the AI agent reading your repository, not a pattern in the code the agent writes. BrassCoders indexes it here because builders shipping AI-augmented code need to know exactly where the deterministic source layer ends and the agent-runtime layer begins. The research below documents the attack; the mitigations live in agent permissioning, sandboxing, and egress control — not in a source scanner, and this index will not pretend otherwise.

[← Back to the index](../README.md) · [Live version](https://coppersun.dev/research/prompt-injection/)

Sources last verified July 2026.

## Sources (5)

---

### 📄 Liu et al. 2025 — "Your AI, My Shell": Demystifying Prompt Injection Attacks on Agentic AI Coding Editors
*arXiv, 2025 (Liu, Zhao, Lyu, Zhang, Wang, Lo)* · [arxiv.org/abs/2509.22040](https://arxiv.org/abs/2509.22040)

BrassCoders treats this as the canonical measurement of how exploitable agentic coding editors are to prompt injection. The authors built AIShellJack, a framework of 314 attack payloads covering 70 MITRE ATT&CK techniques, and ran it against GitHub Copilot and Cursor: attack success rates reached as high as 84% for executing malicious commands after the agent ingested poisoned external content. The objectives ranged from initial access and system discovery to credential theft and data exfiltration. Builders who let an agent auto-run commands against untrusted repositories are exposed at this rate.

- **What it's good for:** the empirical attack-success rate of prompt injection against Copilot and Cursor.
- **Where BrassCoders draws from it:** referenced as the boundary case — the agent-runtime exposure that sits outside BrassCoders's static source-scanning scope.

---

### 📄 OWASP — LLM01:2025 Prompt Injection
*OWASP Top 10 for LLM Applications, 2025* · [owasp.org](https://owasp.org/www-project-top-10-for-large-language-model-applications/)

BrassCoders treats OWASP's LLM01 as the canonical taxonomy entry for prompt injection, which OWASP ranks as the number-one risk in its Top 10 for LLM Applications. The category separates direct injection (a user overriding the system prompt) from indirect injection (malicious instructions embedded in external content the model later reads — a README, an issue, a web page, a dependency's docs). For AI coding agents, indirect injection is the dangerous variant: the payload rides in on the repository the agent was told to work on. Builders standing up an AI-agent workflow should map their exposure against this taxonomy first.

- **What it's good for:** the standard risk taxonomy separating direct from indirect prompt injection.
- **Where BrassCoders draws from it:** complements the LLM-specific risk categories that the source-level scanners in BrassCoders do not, and cannot, cover.

---

### 📊 Willison 2025 — The Lethal Trifecta
*Simon Willison, June 2025* · [simonwillison.net](https://simonwillison.net/2025/Jun/16/the-lethal-trifecta/)

BrassCoders treats Willison's "lethal trifecta" as the clearest mental model for when prompt injection becomes data exfiltration. The trifecta is three capabilities in one agent: access to private data, exposure to untrusted content, and the ability to communicate externally. Any agent with all three can be manipulated by injected instructions into reading secrets and sending them to an attacker. An AI coding agent with repository access, an untrusted dependency or issue in context, and network egress has the full trifecta. Builders configuring agent permissions should treat removing any one leg as the cheapest available mitigation.

- **What it's good for:** the three-capability model that predicts when an agent can be turned into an exfiltration channel.
- **Where BrassCoders draws from it:** the framing BrassCoders uses to explain why the mitigation is agent permissioning, not source scanning.

---

### 📊 Cloud Security Alliance 2026 — AI Agent Prompt Injection: The New CI/CD Supply Chain Threat
*Cloud Security Alliance AI Safety Initiative, June 2026* · [labs.cloudsecurityalliance.org](https://labs.cloudsecurityalliance.org/research/csa-research-note-claude-code-github-action-prompt-injection/)

BrassCoders treats this CSA research note as the canonical documented case of prompt injection escalating to supply-chain compromise through CI. The note dissects an authorization flaw in an AI coding agent's GitHub Action where a permission check trusted any GitHub App actor, letting an unauthenticated external attacker inject a malicious prompt through a single crafted issue and escalate toward repository compromise — without needing write access. Builders wiring AI agents into CI pipelines should read this as concrete proof that agent plus automation plus untrusted input is a live supply-chain vector.

- **What it's good for:** how prompt injection crosses from a developer's editor into the CI/CD supply chain.
- **Where BrassCoders draws from it:** referenced alongside the [dependency-confusion research](https://coppersun.dev/research/dependency-confusion/) as a second AI-era supply-chain vector.

---

### 🔧 Claude Code — Security Model & Prompt-Injection Safeguards
*Anthropic · official documentation* · [code.claude.com/docs/en/security](https://code.claude.com/docs/en/security)

BrassCoders points to Anthropic's Claude Code security documentation as the canonical example of where prompt-injection mitigations actually live: the agent runtime, not the source scanner. The documented safeguards are runtime controls — a permission system requiring explicit approval for sensitive operations, isolated context windows for web fetches so retrieved content cannot inject instructions, trust verification for first-time codebases and new MCP servers, and command-injection detection that forces manual approval of suspicious commands. Builders hardening an AI-agent workflow should configure these controls; a static source scan cannot substitute for them.

- **What it's good for:** the runtime permission-and-sandbox controls that mitigate prompt injection.
- **Where BrassCoders draws from it:** cited as the mitigation layer that complements — and sits outside — BrassCoders's static detection.

## FAQ

### What is prompt injection in an AI coding agent?

Prompt injection is when text the agent reads gets treated as instructions to follow. For coding agents the dangerous form is indirect injection: a malicious instruction hidden in a README, a GitHub issue, a dependency's documentation, or a web page the agent fetches. The agent cannot reliably tell operator instructions from attacker text embedded in the content it was told to work on, so it follows both. OWASP ranks prompt injection as LLM01, the top risk in its Top 10 for LLM Applications.

### Can BrassCoders or any static analyzer detect prompt injection?

No, and this index will not claim otherwise. Prompt injection is a runtime attack on the AI agent, not a pattern in the source code the agent produces — there is nothing in the committed code for a scanner to match. BrassCoders scans source deterministically; prompt-injection mitigations live in agent permissioning, sandboxing, isolated context windows, and egress control. The two operate at different layers and neither substitutes for the other.

### How exploitable are Copilot and Cursor to prompt injection?

Highly, when they auto-run commands against untrusted content. Liu et al. (2025) built AIShellJack — 314 payloads across 70 MITRE ATT&CK techniques — and measured attack-success rates as high as 84% for executing malicious commands on GitHub Copilot and Cursor after the agent ingested poisoned external content. Objectives ranged from initial access to credential theft and data exfiltration.

### What is the lethal trifecta?

Simon Willison's model for when prompt injection becomes data theft: an agent that has (1) access to private data, (2) exposure to untrusted content, and (3) the ability to communicate externally. Any agent with all three can be steered by injected instructions into reading secrets and exfiltrating them. An AI coding agent with repo access, an untrusted issue or dependency in context, and network egress has the full trifecta — and removing any one leg is the cheapest mitigation.

### How do I defend an AI coding agent against prompt injection?

Break the lethal trifecta and add runtime controls. Require explicit approval for command execution rather than auto-approve; sandbox the agent's filesystem and network; isolate the context window used for web fetches so retrieved content cannot inject instructions; restrict network egress; and use trust verification for new codebases and MCP servers. Anthropic's Claude Code security documentation describes a concrete implementation of these controls.

### Can prompt injection reach my CI/CD pipeline?

Yes. The Cloud Security Alliance documented a 2026 case where an AI coding agent's GitHub Action had a permission check that trusted any GitHub App actor, letting an external attacker inject a malicious prompt through a single crafted issue and escalate toward repository compromise without write access. Agent plus automation plus untrusted input is a live supply-chain vector, not a theoretical one.

---

*Generated from the canonical research config. CC BY 4.0. [Live version](https://coppersun.dev/research/prompt-injection/) · [Install BrassCoders](https://coppersun.dev/install/).*
