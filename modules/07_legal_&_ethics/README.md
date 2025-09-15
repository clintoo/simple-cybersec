# Module 7: Legal & Ethics

**Objective:**
This module teaches the legal boundaries, ethical responsibilities, and safe practices every beginner in cybersecurity must follow. It explains what’s legal vs illegal in common jurisdictions, how to responsibly report vulnerabilities, how to get permission to test systems, and how to avoid harm while learning.

> **Important:** This module is educational, not legal advice. Laws vary by country and change over time. If you’re unsure about legality in your jurisdiction, consult a qualified attorney.

---

## 1. Why Legal & Ethical Awareness Matters

* Cybersecurity skills can be used to protect or to cause harm. Knowing the law and following ethical rules keeps you and others safe.
* A simple mistake (scanning or testing a third-party server without permission) can lead to criminal charges, civil liability, job loss, or damaged reputation.
* Ethical behavior builds trust with companies, users, and the security community.

---

## 2. Key Definitions (Simple)

* **White-hat:** A security researcher or practitioner who tests and defends systems with permission and follows disclosure rules.
* **Gray-hat:** Someone who may find vulnerabilities without prior permission and sometimes discloses them; actions can be ethically ambiguous and legally risky.
* **Black-hat:** Malicious actors who exploit vulnerabilities for personal gain or to cause harm.

**Rule of thumb:** If you don’t have explicit permission to test or access a system, treat it as **off-limits**.

---

## 3. Laws you should be aware of (high-level overview)

> Laws differ by country. Below are common examples to illustrate typical rules — read the laws that apply to your location.

### United States — Computer Fraud and Abuse Act (CFAA)

* The CFAA broadly criminalizes unauthorized access to computers and exceeding authorized access.
* Unauthorized scanning, logging in, or extracting data from systems you don’t own can fall under this act.
* Outcomes can include criminal charges and civil lawsuits.

### United Kingdom — Computer Misuse Act (CMA)

* The CMA criminalizes unauthorized access to computer material and unauthorized modification of computer data.
* Intent and authorization are important legal factors; the CMA is commonly used to prosecute hacking.

### European Union — Data Protection & GDPR

* The GDPR regulates processing of personal data (data relating to an identifiable person).
* If your testing collects or exposes personal data (e.g., names, emails, IPs tied to users), you must consider data protection laws and obligations.
* Many countries maintain their own privacy/data protection laws inspired by GDPR.

**Takeaway:** Unauthorized access is commonly illegal; handling personal data can add another legal layer. Always act cautiously.

---

## 4. The Responsible / Coordinated Vulnerability Disclosure (CVD) Approach

Responsible disclosure (also called Coordinated Vulnerability Disclosure, CVD) is the community standard for reporting bugs and vulnerabilities safely:

1. **Do not publish** details publicly until the vendor has had a reasonable time to fix the issue.
2. **Contact the vendor** privately with clear, reproducible steps.
3. **Coordinate** with the vendor on timelines and remediation.
4. **Disclose publicly** only after fixes are available (or according to an agreed timeline) — never leak exploit code that would make users immediately vulnerable.

This approach protects users and gives vendors time to patch while still ensuring issues are reported.

---

## 5. Step-by-step: What to do if you find a vulnerability (safe workflow)

Follow this checklist **immediately** after discovery:

1. **Stop testing** any actions that could damage data, privacy, or service availability.
2. **Document carefully**: record timestamps, exact inputs/requests, screenshots, command output, and how to reproduce.
3. **Collect minimal evidence** — avoid copying or exfiltrating unnecessary personal data.
4. **Do not exploit** the vulnerability for personal gain, to access accounts, or to move laterally inside a target.
5. **Check for a published vulnerability disclosure or security policy** on the vendor’s website (look for “security.txt”, “security”, “vulnerability disclosure policy”, or a contact like [security@company.com](mailto:security@company.com)).
6. **If a policy exists**, follow its instructions (use the provided contact and format).
7. **If no policy exists**, attempt to contact the vendor privately — use a responsible channel (security@, info@, or vendor’s bug bounty platform).
8. **If you cannot reach the vendor**, reach out to a coordinator (CERT/CC or national CERT) for help coordinating a disclosure.
9. **Agree on a disclosure timeline** (vendor may ask for X days to patch). Respect that timeline.
10. **After patching or agreed timeline**, publish a disclosure that praises the vendor for fixing and includes technical details in a way that does not enable attackers (don’t publish exploit code unless absolutely necessary and after coordination).

**If the vendor is unresponsive:** escalate carefully — many researchers use CERT/CC or national CSIRT teams as intermediaries.

---

## 6. Responsible Disclosure: What to include in your report (template)

Below is a simple, reusable email/report template you can use when contacting a vendor’s security team.

```text
Subject: Security report: [short title of issue] — [affected product/version]

Hello [Vendor Security Team],

My name is [Your Name / handle]. I am a security researcher and I discovered a potential vulnerability affecting [product/service name and version]. I am reporting this to allow your team to remediate it before public disclosure.

Summary:
- Affected system: [URL / product / version / service]
- Impact: [brief description — e.g., sensitive data disclosure / remote code execution / authentication bypass]
- Reproducible steps: [step 1, step 2, ...] (include exact requests, parameters, and expected/actual results)
- PoC (optional): [short sanitized proof-of-concept or screenshot, avoid sending live exploit code]
- Evidence: [log snippets, timestamps]

I am happy to coordinate timelines and provide more information. Please let me know a secure channel (PGP key, encrypted email) if you prefer.

I will wait [x days, e.g., 30] for a response before escalating to a coordination body (e.g., CERT) — unless you prefer a different timeline.

Regards,
[Your name / handle]
[contact email]
[PGP key (optional)]
```

**Notes:**

* Use an encrypted channel if the details are sensitive (PGP or vendor-provided secure form).
* Keep copies of your communication and evidence (timestamped).
* Don’t publish details until agreed.

---

## 7. Getting permission: how to request written authorization to test

If you want to perform active testing (scans, fuzzing, exploit attempts), get written permission first. A short authorization message or signed email is usually enough for small-scale academic/research projects. Larger, formal tests may require a contract.

**Simple permission template:**

```text
Subject: Request for written permission to conduct security testing

Hello [Owner Name / Security Team],

I am [name/handle], a security researcher/learner. I would like to perform limited security testing on [target (URL/IP/asset)] during the period [start date — end date].

Scope:
- Tests: [e.g., non-destructive scans, configuration checks, XSS/SQLi testing, no social engineering]
- Tools: [nmap, nikto, burp - list only what you plan to use]
- Goals: [e.g., improve security, find misconfigurations]

I will follow responsible disclosure practices and provide full details of any issues found. I will not access or exfiltrate personal data. Please reply with written permission or let me know if you prefer to decline.

Regards,
[Name]
[contact email]
```

**Tip:** Keep this email thread as the record of authorization if the owner agrees.

---

## 8. Bug bounty platforms and "safe harbor"

Bug bounty platforms (HackerOne, Bugcrowd, GitHub Security Lab, etc.) provide coordinated programs where companies invite researchers to test in-scope targets. These programs typically define:

* Scope (what is allowed).
* Rules of engagement.
* Rewards and recognition.
* Legal terms and sometimes limited legal safe harbor for authorized research.

**If you use a bug-bounty program:** read the rules carefully and follow them exactly — doing so often protects you from legal action for in-scope findings.

**If a company does not have a program:** do not assume a bounty or safe harbor exists — get permission first.

---

## 9. Handling accidental discovery of personal data or secrets

If your testing accidentally reveals personal data (names, emails, passwords) or secret keys:

1. **Stop any further access.**
2. **Do not copy or exfiltrate** the data more than necessary to prove the finding.
3. **Notify the owner securely** (see the Responsible Disclosure template).
4. **If required by law** (e.g., data breach laws), the owner will likely need to report the incident — don’t attempt to "fix" it yourself unless explicitly authorized.

**Tip:** Minimizing data collection reduces legal risk and respects user privacy.

---

## 10. What *not* to do (quick list)

* Don’t run destructive tests (delete data, run ransomware, DDoS) on systems you don’t own.
* Don’t exploit vulnerabilities to access other people’s accounts.
* Don’t publish exploit code without coordination and a strong justification.
* Don’t perform social engineering without explicit, written permission.

Breaking these rules can lead to criminal charges and civil lawsuits.

---

## 11. Evidence, logs & documentation (best practices)

* Keep clean, timestamped logs of what you ran (commands, timestamps, IPs).
* Take screenshots, but avoid including sensitive personal data in shared images.
* When communicating with vendors, use secure channels when available (PGP, vendor forms).
* Keep backups of your original notes and proof of authorization.

Having thorough documentation helps if legal questions arise and improves trust with vendors.

---

## 12. Examples & short scenarios (what to do)

### Scenario A: You find SQL injection on a small website

1. Stop further testing.
2. Record exact steps to reproduce.
3. Find a contact (security.txt, WHOIS, site owner email).
4. Send a responsible disclosure report.
5. Wait for vendor response; if none, escalate to a coordination body.

### Scenario B: You find an exposed admin panel on a public IP

1. Don’t log in.
2. Document the URL and screenshots.
3. Notify the owner and provide reproduction steps.
4. Avoid exploring behind the panel without written permission.

---

## 13. When to involve authorities or a coordinator

* If the vendor ignores your report and the vulnerability is high-risk (e.g., exposes user data), consider escalating to a national CSIRT or CERT (they can help coordinate safely).
* If you suspect criminal activity (active exploit in progress), you may need to contact law enforcement — but do this carefully and document your findings.

---

## 14. Ethics beyond the law

* **Respect privacy:** just because data is accessible doesn’t mean you should read or copy it.
* **Avoid harm:** the goal of security research is to make systems safer, not to prove you can break them.
* **Credit and humility:** when publishing responsibly, acknowledge the vendor and give them time to fix issues.

---

## 15. Further reading & helpful links

* CERT Coordination Center — *Guide to Coordinated Vulnerability Disclosure* (recommended reading)
* CISA — Coordinated Vulnerability Disclosure Program resources
* Official legal texts for your jurisdiction (e.g., GDPR, CFAA, Computer Misuse Act)
* Bug bounty platforms: HackerOne, Bugcrowd, GitHub Security Lab
