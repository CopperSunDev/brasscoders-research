# Will My AI Use pickle.load Or yaml.load On Untrusted Data?

> The CVE track record on Python and ML-framework deserialization risk — real vulnerabilities in pickle, PyYAML, and model-loading code, plus the tools that catch the pattern every builder shipping AI-generated Python should know.

BrassCoders treats the CVE history on Python deserialization as the load-bearing evidence for this category: the bug class is a decade old, and AI-generated code keeps reintroducing it inside newer abstractions. PyYAML's yaml.load carried a CVSS-9.8 CVE in 2017; PyTorch's torch.load carried a CVSS-9.3 CVE in 2025 for the exact same underlying weakness, CWE-502, three layers deeper inside a framework wrapper. The resources below are the primary-source record — CVE advisories, the standard library's own warning, and the scanners built specifically to catch it.

[← Back to the index](../README.md) · [Live version](https://coppersun.dev/research/insecure-deserialization/)

Sources last verified August 2026.

## Sources (8)

---

### 📄 CWE-502 — Deserialization of Untrusted Data
*MITRE, CWE List* · [cwe.mitre.org](https://cwe.mitre.org/data/definitions/502.html)

BrassCoders treats CWE-502 as the canonical taxonomy entry for the entire deserialization-of-untrusted-data vulnerability class. MITRE defines it as a product reconstructing objects from serialized data without confirming the data's validity first, and names gadget-chain attacks as the mechanism that turns a parsing bug into arbitrary code execution. Builders auditing AI-generated Python that reads a file, a queue message, or a cached object should treat any pickle.load, yaml.load, or model-loading call as a CWE-502 candidate until proven otherwise.

- **What it's good for:** defining the vulnerability class and the gadget-chain attack mechanism that makes it exploitable.
- **Where BrassCoders draws from it:** the taxonomy Bandit's B301 and B506 rules encode as pattern checks inside BrassCoders.

---

### 🏢 CVE-2017-18342 — PyYAML Arbitrary Code Execution
*GHSA-rprw-h62v-c2w7, published Jan 2019* · [github.com/advisories/GHSA-rprw-h62v-c2w7](https://github.com/advisories/GHSA-rprw-h62v-c2w7)

BrassCoders treats CVE-2017-18342 as the reference case for why yaml.load is unsafe by default. PyYAML versions before 5.1 let yaml.load execute arbitrary code on crafted input, network-exploitable with no authentication and no user interaction required — the advisory carries a 9.8 CVSS score, close to the ceiling of the scale. The fix wasn't a patch to yaml.load itself; it was PyYAML 5.1 changing the default Loader and pushing developers toward yaml.safe_load.

- **What it's good for:** the exact PyYAML version range, exploitation preconditions, and the CVSS 9.8 severity rating.
- **Where BrassCoders draws from it:** the B506 pattern check BrassCoders' Bandit integration runs against every yaml.load call site.

---

### 🏢 CVE-2025-32434 — PyTorch torch.load weights_only Bypass
*GHSA-53q9-r3pm-6pq6, published Apr 2025* · [github.com/pytorch/pytorch/security/advisories](https://github.com/pytorch/pytorch/security/advisories/GHSA-53q9-r3pm-6pq6)

BrassCoders treats CVE-2025-32434 as proof that a documented safe mode can still ship a CWE-502 hole. PyTorch's own documentation recommended torch.load(weights_only=True) as the safe way to load an untrusted checkpoint; the CVSS-9.3 advisory shows that flag alone did not stop remote code execution on PyTorch 2.5.1 and earlier. Builders who copied AI-generated model-loading code that sets weights_only=True should confirm the PyTorch version, not just the flag.

- **What it's good for:** how a documented mitigation flag failed to close the deserialization path, and the affected version range.
- **Where BrassCoders draws from it:** why BrassCoders flags any torch.load call for triage rather than trusting the presence of weights_only=True as sufficient.

---

### 🏢 CVE-2026-12484 — Keras TorchModuleWrapper Unsafe Pickle Load
*GHSA-v2w2-w228-c444, Keras* · [github.com/advisories/GHSA-v2w2-w228-c444](https://github.com/advisories/GHSA-v2w2-w228-c444)

BrassCoders treats CVE-2026-12484 as evidence the deserialization risk moved into the ML framework's own convenience wrappers, not just raw pickle calls. Keras's TorchModuleWrapper.from_config method calls torch.load with weights_only=False by default outside an explicit safe-mode context, a CVSS-7.8 hole patched in Keras 3.12.3 and 3.15.0. The vulnerable code path never says the word pickle; it sits three layers removed from the primitive builders are taught to watch for.

- **What it's good for:** how an ML-framework convenience wrapper reintroduced unsafe deserialization behind an abstraction layer.
- **Where BrassCoders draws from it:** the case for flagging deserialization patterns at every layer, not only the outermost pickle.load or yaml.load call.

---

### 📊 JFrog — Malicious Hugging Face ML Models With Silent Backdoor
*JFrog Security Research, Feb 2024* · [jfrog.com](https://jfrog.com/blog/data-scientists-targeted-by-malicious-hugging-face-ml-models-with-silent-backdoor/)

BrassCoders treats this as the canonical evidence that pickle-based model deserialization is an active exploitation vector, not a theoretical one. JFrog's security research team found close to 100 malicious models on Hugging Face carrying real payloads, PyTorch pickle files the most common carrier, including one that opened a reverse shell to an attacker-controlled address on load. Builders who write or accept AI-generated code that downloads a Hugging Face checkpoint and loads it directly are one torch.load call away from running someone else's shell command.

- **What it's good for:** real malicious models found in the wild on Hugging Face and the reverse-shell payload mechanism.
- **Where BrassCoders draws from it:** the argument that untrusted model files deserve the same suspicion as any other untrusted input reaching pickle.load or torch.load.

---

### 📄 Python Documentation — pickle Module Security Warning
*Python Software Foundation, docs.python.org* · [docs.python.org/3/library/pickle.html](https://docs.python.org/3/library/pickle.html)

BrassCoders treats the pickle module's own documentation as the most unimpeachable source in this category — the standard library warns about itself. The docs state plainly that pickle is not secure and that unpickling data from an untrusted source can execute arbitrary code, then point to json or an hmac signature as the safer path. When an AI assistant reaches for pickle.load on data whose origin the prompt didn't specify, it contradicts a warning three sentences long in the library it just imported.

- **What it's good for:** the exact standard-library warning text and the json / hmac alternatives it recommends.
- **Where BrassCoders draws from it:** the baseline BrassCoders' Bandit B301 rule enforces on every pickle.load, pickle.loads, and cPickle call.

---

### 📄 OWASP Deserialization Cheat Sheet
*OWASP Foundation* · [cheatsheetseries.owasp.org](https://cheatsheetseries.owasp.org/cheatsheets/Deserialization_Cheat_Sheet.html)

BrassCoders treats the OWASP Deserialization Cheat Sheet as the canonical practitioner reference for closing CWE-502 across languages. The sheet names pickle, c_pickle, and PyYAML's load method as Python's specific danger patterns, then gives two language-agnostic defenses: prefer a plain data format like JSON over native serialization, and sign serialized data so tampered input gets rejected before deserialization runs. Builders remediating a BrassCoders finding on pickle or yaml.load should start here for the fix pattern, not just the flag.

- **What it's good for:** Python-specific danger patterns and the two general defenses — avoid native serialization, or sign the data.
- **Where BrassCoders draws from it:** the remediation guidance BrassCoders' YAML output points builders toward for B301 and B506 findings.

---

### 🔧 ModelScan
*Protect AI · Python · open source* · [github.com/protectai/modelscan](https://github.com/protectai/modelscan)

BrassCoders treats ModelScan as the canonical open-source scanner for the model-file half of this problem — the part a source-code SAST tool doesn't reach. Protect AI's tool reads Pickle, SavedModel, and H5 model files byte-by-byte to flag unsafe code signatures across PyTorch, TensorFlow, Keras, Sklearn, and XGBoost without executing the file. BrassCoders' own scanners catch pickle.load and torch.load call sites in source code; ModelScan catches the payload already sitting inside a model file someone downloaded.

- **What it's good for:** static scanning of serialized model files themselves, independent of the calling source code.
- **Where BrassCoders draws from it:** complementary coverage BrassCoders doesn't claim — BrassCoders flags the unsafe call site in source, not the contents of a binary model file.

## FAQ

### What is CWE-502 and why does it matter for AI-generated Python?

CWE-502, Deserialization of Untrusted Data, is MITRE's name for a class of bugs where a program reconstructs objects from serialized input without checking whether that input is trustworthy. AI coding assistants generate pickle.load, yaml.load, and model-loading calls as often as any other API, and doing so without a check on where the data comes from turns the call into a CWE-502 vulnerability.

### Has pickle.load ever caused a real CVE?

Yes, though the vulnerability usually gets attributed to whatever ships the deserialization call rather than to pickle.load itself. CVE-2017-18342 hit PyYAML's yaml.load at CVSS 9.8, and more recent CVEs like CVE-2025-32434 in PyTorch's torch.load and CVE-2026-12484 in Keras's TorchModuleWrapper show the same bug class reappearing inside ML-framework model loading a decade later.

### Is torch.load(weights_only=True) actually safe?

Not on its own. CVE-2025-32434, CVSS 9.3, showed that PyTorch's documented mitigation — setting weights_only=True — still allowed remote code execution on PyTorch 2.5.1 and earlier. The fix was upgrading PyTorch to 2.6.0, not adding the flag; builders should confirm both the flag and the version.

### Does BrassCoders detect unsafe deserialization?

BrassCoders' Bandit integration flags pickle.load, pickle.loads, and cPickle calls under rule B301, and unsafe yaml.load calls under rule B506, by pattern on every scan. BrassCoders does not determine whether the specific input is actually untrusted — that context call belongs to the AI triage layer reading the surrounding code, per the product's brass-reports-Claude-triages division of labor.

### Are malicious ML models actually being distributed, or is this theoretical?

It's active, not theoretical. JFrog's security research team found close to 100 malicious models on Hugging Face carrying real payloads in February 2024, PyTorch pickle files the most common carrier, including one model that opened a reverse shell to an attacker's server the moment it loaded.

### What should I use instead of pickle or yaml.load for untrusted data?

The OWASP Deserialization Cheat Sheet gives two general fixes: prefer a plain data format like JSON that can't encode executable objects, or cryptographically sign serialized data and reject anything unsigned before deserializing it. For YAML specifically, yaml.safe_load is a one-word swap that removes the code-execution path entirely.

---

*Generated from the canonical research config. CC BY 4.0. [Live version](https://coppersun.dev/research/insecure-deserialization/) · [Install BrassCoders](https://coppersun.dev/install/).*
