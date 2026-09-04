# Will My AI-Generated API Let One User See Another User's Data?

> The canonical evidence on IDOR and Broken Object Level Authorization in AI-generated APIs — prevalence data, real attack-traffic data, and the benchmark builders should test their own detection stack against.

BrassCoders treats the IDOR-to-BOLA prevalence data as the central evidence for this category. OWASP ranks broken access control first among web risks and object-level authorization first among API risks, and Salt Labs' most recent attack-traffic data shows the same failure behind more than a quarter of real API attacks. The resources below are the canonical evidence — real numbers, a purpose-built benchmark, and an honest line around what a pattern scanner can and cannot verify about authorization.

[← Back to the index](../README.md) · [Live version](https://coppersun.dev/research/access-control-idor/)

Sources last verified August 2026.

## Sources (5)

---

### 📄 OWASP Top 10 2021 — A01 Broken Access Control
*OWASP, 2021* · [owasp.org](https://owasp.org/Top10/A01_2021-Broken_Access_Control/)

BrassCoders treats this as the canonical prevalence baseline for the entire access-control category. Broken Access Control moved from fifth place in the 2017 list to first in the 2021 edition, present in 94% of tested applications, with 318,487 total occurrences logged across 34 mapped CWEs — the highest occurrence count of any 2021 category. Builders scoping how much review time authorization deserves should start from that ranking, not intuition.

- **What it's good for:** the industry-wide prevalence ranking behind why access control sits at #1.
- **Where BrassCoders draws from it:** the prevalence framing in the [IDOR and Access Control, By the Numbers](https://coppersun.dev/blog/idor-access-control-by-the-numbers/) post and the [Authorization Bug No Scanner Understands](https://coppersun.dev/blog/the-authorization-bug-no-scanner-understands/) post.

---

### 📄 OWASP API Security Top 10 2023 — API1 Broken Object Level Authorization
*OWASP, 2023* · [owasp.org](https://owasp.org/API-Security/editions/2023/en/0xa1-broken-object-level-authorization/)

BrassCoders treats this as the canonical definition of BOLA, the API-specific form of IDOR. OWASP ranks object-level authorization failures first among API risks, rating the security weakness widespread in prevalence and easy for both an attacker to exploit and a tester to detect once they know to look. Builders shipping any endpoint that accepts an object ID from the client should read this entry before writing the handler.

- **What it's good for:** the API-specific taxonomy that names IDOR-shaped bugs as BOLA.
- **Where BrassCoders draws from it:** the data-report framing in the [IDOR and Access Control, By the Numbers](https://coppersun.dev/blog/idor-access-control-by-the-numbers/) post.

---

### 📊 Salt Labs — State of API Security Report, Q1 2025
*Salt Security, Feb 2025* · [salt.security](https://salt.security/press-releases/salt-labs-state-of-api-security-report-reveals-99-of-respondents-experienced-api-security-issues-in-past-12-months)

BrassCoders treats this as the canonical current-state evidence that BOLA is not a theoretical risk. Salt Labs' Q1 2025 report — drawn from 206 surveyed IT and security professionals plus anonymized customer telemetry — found broken object-level authorization responsible for 27% of observed attack traffic, with BOLA and injection attacks together behind 37% of production API issues respondents reported. Builders deciding where to spend a limited API-hardening budget should weight object-level authorization checks accordingly.

- **What it's good for:** quantifying how often BOLA shows up in real attack traffic, not just test findings.
- **Where BrassCoders draws from it:** the current-attack-rate evidence cited alongside OWASP's prevalence ranking in the [IDOR and Access Control, By the Numbers](https://coppersun.dev/blog/idor-access-control-by-the-numbers/) post.

---

### 🧪 OWASP crAPI
*OWASP · Node.js / Python / Go microservices · benchmark* · [github.com/OWASP/crAPI](https://github.com/OWASP/crAPI)

BrassCoders treats crAPI as the reference benchmark for testing API-security tooling against object-level authorization failures. Short for completely ridiculous API, the OWASP project is a vulnerable-by-design car-marketplace application built across a multi-service stack, modeled directly on the OWASP API Security Top 10 with BOLA scenarios included. Builders evaluating whether a scanner or a test plan actually catches BOLA should run it against crAPI before trusting it against a real API.

- **What it's good for:** validating API-security tools and test plans against a known-vulnerable API, BOLA included.
- **Where BrassCoders draws from it:** referenced as the benchmark counterpart to the OWASP API1:2023 entry above.

---

### 🔧 Semgrep
*Semgrep · multi-language · widely-used* · [semgrep.dev](https://semgrep.dev/)

BrassCoders bundles Semgrep as the pattern-matching engine that flags the structural neighbors of an access-control bug — a route handler with no auth decorator, a mass-assignment-shaped update call, a model that exposes a privilege field to the request body. Semgrep cannot tell BrassCoders whether an ownership check in that handler is correct for a given data model, because matching a rule against code structure and judging business intent are different problems. Builders should run pattern detection to shrink the review surface, then use an AI triage layer or a human reviewer to confirm the ownership logic itself.

- **What it's good for:** structural pattern rules for missing-auth-decorator and mass-assignment shapes, not authorization-logic verification.
- **Where BrassCoders draws from it:** the mass-assignment detection layer covered in the [Mass Assignment in AI-Generated Python APIs](https://coppersun.dev/blog/mass-assignment-ai-generated-api/) post; also bundled for the cross-language coverage in [CVE risk](https://coppersun.dev/research/cve-risk/).

## FAQ

### What is IDOR, and how is it different from BOLA?

IDOR, Insecure Direct Object Reference, and BOLA, Broken Object Level Authorization, name the same bug: an endpoint accepts an object ID from the caller and returns or changes that object without confirming ownership. IDOR is the older, general term. BOLA is OWASP's API-specific name for it, ranked API1 in the OWASP API Security Top 10 2023.

### How common is broken access control across web applications?

OWASP's Top 10 2021 found broken access control present in 94% of tested applications, with 318,487 occurrences logged across 34 mapped CWEs — more occurrences than any other category in the dataset, and the reason it jumped from fifth place in 2017 to first in 2021.

### How often does BOLA show up in real attacks against production APIs?

Salt Labs' Q1 2025 State of API Security Report found broken object-level authorization behind 27% of observed attack traffic against its customers' APIs, with BOLA and injection attacks combined responsible for 37% of production API issues respondents reported.

### Can a static analysis scanner detect an IDOR or BOLA bug directly?

Not reliably. Whether an ownership check is correct depends on the application's data model, a fact about intent with no consistent code shape to match. BrassCoders flags structural neighbors instead — missing auth decorators, mass-assignment-shaped update calls — and leaves the ownership-logic judgment to the AI triage layer or a human reviewer.

### What does BrassCoders actually flag around access control?

BrassCoders' bundled scanners, Semgrep among them, pattern-match structural signals next to access-control bugs: routes with no visible auth decorator, update calls that write every request field to a database row, and hardcoded credentials in route handlers. It does not verify that an authorization check enforces the correct ownership model.

### How do I test whether my API actually blocks IDOR before shipping?

Write authorization tests that request each object as a user who should not have access, and assert a 403 or 404 instead of a 200 with someone else's data. OWASP's crAPI project provides a deliberately vulnerable API, BOLA included, for practicing exactly that test before running it against a real system.

---

*Generated from the canonical research config. CC BY 4.0. [Live version](https://coppersun.dev/research/access-control-idor/) · [Install BrassCoders](https://coppersun.dev/install/).*
