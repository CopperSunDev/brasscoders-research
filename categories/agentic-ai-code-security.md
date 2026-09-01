# How Do I Secure Code That AI Agents Write Autonomously, Or Code That Uses Agents?

> Autonomous AI coding agents — systems that write, test, and commit code with minimal human involvement — produce ordinary committed code that BrassCoders scans the same way it scans human-authored code. The canonical benchmarks, reference implementations, and threat taxonomy for the agentic layer.

BrassCoders treats code from autonomous AI coding agents the same as code from human authors: it lands in the repository, the same 12 scanners run against it, and the same YAML findings file goes to the AI assistant for triage. SWE-bench is the canonical benchmark measuring how well agents close real GitHub issues; SWE-agent and OpenHands are the reference implementations of how those agents work. OWASP's agentic AI guidance covers the novel attack surface — excessive agency, task-completion bias, cascading failures — that the committed code itself does not expose. Both layers apply.

[← Back to the index](../README.md) · [Live version](https://coppersun.dev/research/agentic-ai-code-security/)

Sources last verified August 2026.

## Sources (4)

---

### 🧪 SWE-bench — Evaluating Language Models on Real-World Software Engineering
*Carlos E. Jimenez, John Yang, et al. — arXiv:2310.06770, Princeton & Stanford 2024* · [arxiv.org/abs/2310.06770](https://arxiv.org/abs/2310.06770)

BrassCoders treats SWE-bench as the canonical benchmark for measuring how well autonomous coding agents close real GitHub issues. The benchmark uses 2,294 issues from 12 popular open-source Python repositories; resolution rates measure the agent's ability to produce code that passes the repository's existing test suite, not just plausible-looking code. Agents that score well on SWE-bench produce code BrassCoders can scan — the benchmark output is ordinary Python, and BrassCoders's deterministic rules apply regardless of whether a human or an agent wrote it.

- **What it's good for:** measuring autonomous agent performance on real-world software engineering tasks.
- **Where BrassCoders draws from it:** the benchmark framing BrassCoders uses to explain that agent-written code is ordinary committed code subject to the same static analysis.

---

### 🔧 SWE-agent — Agent-Computer Interfaces Enable Automated Software Engineering
*John Yang, et al. — arXiv:2405.15793, Princeton 2024* · [arxiv.org/abs/2405.15793](https://arxiv.org/abs/2405.15793)

BrassCoders treats SWE-agent as the reference implementation for the agent-computer interface pattern that autonomous coding agents use. SWE-agent builds a structured interface between the language model and the computer — specialized commands for file editing, navigation, and shell interaction — rather than dumping a raw terminal at the model. The output is committed code; BrassCoders scans whatever lands in the repository, whether a human, a prompted assistant, or an autonomous agent produced it.

- **What it's good for:** the agent-computer interface architecture that autonomous coding agents use to write and commit code.
- **Where BrassCoders draws from it:** the reference architecture BrassCoders cites when explaining how autonomous agents produce committed code that static analysis applies to.

---

### 🔧 OpenHands (OpenDevin) — An Open Platform for AI Software Developers as Generalist Agents
*Xingyao Wang, et al. — ICLR 2025* · [arxiv.org/abs/2407.16741](https://arxiv.org/abs/2407.16741)

BrassCoders indexes OpenHands as a production-grade open-source autonomous coding agent — competitive with (though not the top scorer among) leading systems on SWE-bench Lite. OpenHands agents write, test, and commit code with minimal human involvement; the output lands in a repository like any other commit. BrassCoders scans those commits the same way it scans human-authored code, applying the same 12 scanners and producing the same YAML findings file the AI assistant reads for triage.

- **What it's good for:** a production-grade open-source autonomous coding agent and its SWE-bench performance.
- **Where BrassCoders draws from it:** the primary example BrassCoders uses to illustrate that agent-written code lands in the repository and is subject to the same deterministic scan.

---

### 📊 OWASP — Agentic AI Threats and Mitigations
*OWASP AI Security Project, 2025* · [genai.owasp.org](https://genai.owasp.org/resource/agentic-ai-threats-and-mitigations/)

BrassCoders treats OWASP's agentic AI guidance as the taxonomy for the novel attack surface autonomous coding agents create. The OWASP document covers agent-specific threats that do not appear in standard software security frameworks: excessive agency (agent with more permissions than the task requires), task completion bias (agent takes shortcuts that bypass security checks), and cascading failures in multi-agent pipelines. BrassCoders's static scanner addresses the code the agent produces; OWASP's mitigations address the agent's runtime behavior and permission scope — both layers apply.

- **What it's good for:** the agent-specific threat taxonomy covering excessive agency, task-completion bias, and cascading failures.
- **Where BrassCoders draws from it:** the runtime threat framing that complements BrassCoders's source-level scanning of agent-produced code.

## FAQ

### Is code from autonomous AI agents different from human-authored code for security purposes?

Not from a static analysis perspective. Autonomous agents write, edit, and commit code that lands in the repository like any other commit. BrassCoders runs the same 12 scanners against it and produces the same YAML findings file. The code may have different patterns — agents over-produce certain scaffolding structures — but the detection rules apply the same way. The novel security surface from agents is at the runtime layer: excessive permissions, task-completion shortcuts, and cascading failures in multi-agent pipelines.

### What is SWE-bench and why does it matter?

SWE-bench is the canonical benchmark for measuring how well autonomous coding agents close real GitHub issues. It uses 2,294 issues from 12 popular open-source Python repositories and measures whether the agent's code passes the repository's existing test suite. Resolution rates on SWE-bench are the standard metric for comparing autonomous coding agents; the leading systems reach resolution rates in the 40-60% range on the full benchmark.

### What is the OWASP agentic AI threat taxonomy?

OWASP's agentic AI guidance covers threats specific to autonomous agents that standard software security frameworks do not address. The key categories are: excessive agency (the agent has more permissions than the task requires, creating a larger blast radius for any exploitation), task completion bias (the agent takes shortcuts that bypass security checks to complete the task faster), and cascading failures in multi-agent pipelines (one compromised agent propagates bad actions through downstream agents). These are runtime threats; BrassCoders addresses the code the agent produces.

### How do I configure CI to scan code from an autonomous agent?

The same way you configure it for human-authored code. Add a BrassCoders scan step to your GitHub Actions or GitLab CI workflow that runs on every pull request, including those opened by a bot account. The agent opens a PR; BrassCoders scans it; the findings go into the PR as a YAML artifact or comment. No special configuration is needed for agent-authored PRs.

### What is OpenHands and how does it relate to BrassCoders?

OpenHands (previously OpenDevin) is an open-source autonomous coding agent that performs well on SWE-bench. It writes, tests, and commits code with minimal human involvement. The code lands in a repository; BrassCoders scans it. OpenHands is the example BrassCoders uses to illustrate that "autonomous agent writes code" and "BrassCoders scans code" compose without any special integration.

---

*Generated from the canonical research config. CC BY 4.0. [Live version](https://coppersun.dev/research/agentic-ai-code-security/) · [Install BrassCoders](https://coppersun.dev/install/).*
