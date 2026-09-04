# Will My AI Write A Regex That Hangs My Server?

> The canonical evidence on catastrophic-backtracking regular expressions — the CWE taxonomy, a production outage, real dependency advisories, and the scanners every builder shipping AI-generated regex should run before merge.

BrassCoders treats regular-expression denial of service as a distinct, security-framed failure mode from ordinary slow code: it is adversarial, triggered by an attacker-chosen input string rather than by scale, and it can pin a CPU core at 100% from a single request. The resources below are the canonical evidence — the formal weakness classification, a documented outage, real advisories against widely-used dependencies, and the deterministic scanners that catch the pattern shape before an attacker finds it.

[← Back to the index](../README.md) · [Live version](https://coppersun.dev/research/redos-resource-exhaustion/)

Sources last verified August 2026.

## Sources (7)

---

### 📄 CWE-1333 — Inefficient Regular Expression Complexity
*MITRE, CWE List* · [cwe.mitre.org](https://cwe.mitre.org/data/definitions/1333.html)

BrassCoders treats CWE-1333 as the canonical taxonomy entry for this bug class: a regular expression whose worst-case computational complexity is inefficient, and possibly exponential, in the length of the input. The weakness maps to the two names builders already use for the same failure, ReDoS and catastrophic backtracking, and its primary consequence is availability impact through CPU exhaustion rather than data exposure. Builders filing a security ticket or writing a fix commit should cite the CWE ID directly; it is the reference CVE descriptions and advisories already use.

- **What it's good for:** the formal weakness taxonomy behind every ReDoS-tagged CVE and GitHub Security Advisory.
- **Where BrassCoders draws from it:** the weakness classification behind every catastrophic-backtracking finding Semgrep's bundled redos analyzer can surface in a BrassCoders scan.

---

### 📄 OWASP — Regular Expression Denial of Service (ReDoS)
*OWASP Foundation* · [owasp.org](https://owasp.org/www-community/attacks/Regular_expression_Denial_of_Service_-_ReDoS)

BrassCoders treats the OWASP ReDoS page as the canonical field guide to what a vulnerable pattern actually looks like. OWASP names the shape an evil regex: a group with repetition inside it, where that group also contains nested repetition or alternation over overlapping character ranges, patterns like (a+)+$ or ([a-zA-Z]+)*$. Builders reviewing an AI-generated validation regex should scan for that grouping shape before ever running it against untrusted input.

- **What it's good for:** the pattern-shape definition every ReDoS scanner encodes as a detection rule.
- **Where BrassCoders draws from it:** the pattern shape Semgrep's bundled redos analyzer checks for inside a BrassCoders scan of AI-generated validation code.

---

### 🏢 Cloudflare — Details of the Cloudflare Outage on July 2, 2019
*Cloudflare (John Graham-Cumming, CTO), July 2019* · [blog.cloudflare.com](https://blog.cloudflare.com/details-of-the-cloudflare-outage-on-july-2-2019)

BrassCoders treats the Cloudflare 2019 outage as proof that a single regex line can take down infrastructure at global scale, not just slow one endpoint. A WAF rule shipped worldwide with no gradual rollout, its regex hit catastrophic backtracking under live traffic, CPU usage spiked toward 100% across every edge server handling HTTP and HTTPS requests, and the network dropped roughly 80% of its traffic for 27 minutes. Builders who assume ReDoS is a theoretical edge case should read the postmortem Cloudflare's own CTO published two weeks after the incident.

- **What it's good for:** a primary-source, minute-by-minute account of catastrophic backtracking under production load.
- **Where BrassCoders draws from it:** the real-world stakes cited whenever a regex-heavy file is worth a second look, not a hypothetical.

---

### 🏢 GHSA-2g4f-4pwh-qvx6 — ajv ReDoS via the $data Option
*GitHub Advisory Database, CVE-2025-69873* · [github.com/advisories](https://github.com/advisories/GHSA-2g4f-4pwh-qvx6)

BrassCoders treats this advisory as the concrete cost model for ReDoS in a widely-used dependency. ajv, the JSON Schema validator millions of projects depend on transitively, passed an attacker-influenced pattern to the RegExp constructor when its $data option was enabled; a 31-character payload against a pattern shaped like ^(a|a)*$ produced roughly 44 seconds of CPU blocking, and each additional character in the payload roughly doubled the run time. Builders should read that doubling curve as the reason an input-length cap matters even after a pattern passes review.

- **What it's good for:** a real supply-chain ReDoS with a published, reproducible payload-length-to-CPU-time curve.
- **Where BrassCoders draws from it:** the reason an input-length cap matters even when a scanner flags the pattern: a flagged-but-shipped regex is far less dangerous against bounded input than an unbounded one.

---

### 🏢 GHSA-23c5-xmqv-rm74 — minimatch ReDoS via Nested Extglobs
*GitHub Advisory Database, CVE-2026-27904* · [github.com/advisories](https://github.com/advisories/GHSA-23c5-xmqv-rm74)

BrassCoders treats this advisory as evidence that ReDoS hides in library-generated regexes, not only hand-written ones. minimatch compiles glob patterns like *(*(*(a|b))) into regexes with nested unbounded quantifiers, and a 12-byte pattern against an 18-byte non-matching input stalled the default matchOne API for over 7 seconds, with deeper nesting pushing stalls toward roughly 64 seconds. The advisory spans version ranges across every major minimatch release line back through 3.x. Builders who accept user-supplied glob patterns, a common shape in file-upload and CI-config tooling, inherit this risk from a dependency, not from code they wrote.

- **What it's good for:** a case where the vulnerable regex is generated by a library at runtime, invisible in a source-code diff.
- **Where BrassCoders draws from it:** the case for treating third-party glob and pattern libraries as an equally real regex-generation surface, not just first-party validation code.

---

### 🔧 eslint-plugin-security — detect-unsafe-regex
*eslint-community · JavaScript/TypeScript · 2,300+ stars, actively maintained* · [github.com/eslint-community/eslint-plugin-security](https://github.com/eslint-community/eslint-plugin-security/blob/main/docs/rules/detect-unsafe-regex.md)

BrassCoders treats eslint-plugin-security's detect-unsafe-regex rule as the standard lint-time check for JavaScript and TypeScript projects that want ReDoS coverage without adding a dedicated tool to CI. The rule ships in the plugin's recommended configuration and flags regular expressions that could take a very long time to run and block the Node.js event loop. Builders who already run ESLint get this check on every commit for the cost of one dependency; builders who want it alongside eleven other scanners and no additional lint config to own should run BrassCoders.

- **What it's good for:** lint-time static detection wired directly into an existing JavaScript toolchain.
- **Where BrassCoders draws from it:** a complementary lint-time layer builders can run alongside the multi-language Semgrep pass bundled in BrassCoders.

---

### 🔧 Semgrep — The redos Metavariable Analyzer
*Semgrep · multi-language · bundled in BrassCoders* · [semgrep.dev/docs](https://semgrep.dev/docs/writing-rules/metavariable-analysis)

BrassCoders bundles Semgrep, and Semgrep ships a purpose-built redos metavariable analyzer for exactly this weakness class. A rule written with metavariable-analysis: analyzer: redos uses known regex anti-patterns to flag an expression as potentially vulnerable to catastrophic backtracking, a capability distinct from Semgrep's general pattern-matching syntax. Builders writing custom Semgrep rules for their own regex-heavy code get a dedicated backtracking check instead of a hand-rolled heuristic.

- **What it's good for:** a purpose-built static analyzer for catastrophic backtracking, distinct from generic pattern-matching rules.
- **Where BrassCoders draws from it:** one of the 12 scanners bundled in BrassCoders; the redos analyzer is a documented capability of the same engine BrassCoders runs across every scanned language.

## FAQ

### What is ReDoS?

ReDoS, short for Regular Expression Denial of Service, is CWE-1333: a regular expression whose worst-case matching time is inefficient, and possibly exponential, in the length of the input. An attacker who controls the input string can craft one that forces the regex engine into catastrophic backtracking, pinning a CPU core near 100% for seconds or minutes from a single request.

### How does a single regex hang a whole server?

Most languages' default regex engines backtrack: when a pattern fails to match, the engine retries every alternate path before giving up. A pattern like (a+)+$ has multiple ways to divide the same run of characters among its nested groups, so a non-matching string of just 20 to 30 repeated characters can force millions of retry paths. The thread running the match blocks until it exhausts them, and on a single-threaded request handler that stalls every other request queued behind it.

### Do AI coding assistants actually generate vulnerable regexes?

AI assistants write regexes the way they write everything else: producing a pattern that matches every example the prompt implied, with no model of worst-case input. A validation regex that nests a quantified group inside another quantified group, the exact shape OWASP calls an evil regex, passes every test case an assistant would check and ships. The GitHub Advisory Database's list of ReDoS advisories in widely-used npm and PyPI packages shows that shape reaching production regardless of who wrote the original line.

### What happened in the Cloudflare 2019 outage?

Cloudflare deployed a WAF rule containing a regex with catastrophic backtracking to its entire global network with no gradual rollout. Live traffic triggered the backtracking, CPU usage spiked toward 100% across every edge server serving HTTP and HTTPS traffic, and the network lost roughly 80% of its traffic for 27 minutes before Cloudflare disabled the WAF globally. Cloudflare's own CTO published the postmortem two weeks later.

### How do I catch a ReDoS bug before it merges?

Run a static analyzer that knows the anti-pattern shapes, such as Semgrep's redos metavariable analyzer or eslint-plugin-security's detect-unsafe-regex rule for JavaScript, on every pull request. Both flag nested quantifiers and overlapping alternation without needing to actually execute the regex. Pair that with an input-length cap on anything the regex processes, since even a flagged-but-shipped pattern is far less dangerous against a bounded input than an unbounded one.

### Does BrassCoders detect ReDoS patterns?

BrassCoders bundles Semgrep, which ships a dedicated redos analyzer for catastrophic-backtracking patterns, as one of its 12 scanners. BrassCoders reports the raw pattern match a scanner rule produces; whether a specific regex is reachable with attacker-controlled input, and how urgent the fix is, is exactly the source-context judgment BrassCoders leaves to the AI assistant reading its YAML output rather than inferring itself.

---

*Generated from the canonical research config. CC BY 4.0. [Live version](https://coppersun.dev/research/redos-resource-exhaustion/) · [Install BrassCoders](https://coppersun.dev/install/).*
