# Threat Modeling: Aaronic Oil & Energy Website

## What We Were Solving

Aaronic Oil & Energy, an e-commerce platform in the oil and energy sector, needed to understand its security risks before a major launch. The question: "What could go wrong, and how do we prevent it?" This project involved conducting a comprehensive threat analysis identifying every possible attack vector and documenting mitigation strategies for each one.


## Our Approach

Collaborated with a team of 8 security professionals through a **STRIDE-based threat modeling process**:

1. **System Analysis:** Mapped the target system (frontend UI, backend API, database, cloud storage, payment processor)
2. **Threat Identification:** Applied STRIDE categories to each component
3. **Risk Assessment:** Scored threats by likelihood and impact
4. **Mitigation Planning:** Documented controls and compensating measures for each threat
5. **Documentation:** Created multiple deliverables for different stakeholders


## Framework Applied

**STRIDE Methodology** breaks threats into 6 categories:

- **S**poofing Identity: Can attackers impersonate users or systems?
- **T**ampering with Data: Can attackers modify data in transit or at rest?
- **R**epudiation: Can attackers deny their actions?
- **I**nformation Disclosure: Can attackers access sensitive data?
- **D**enial of Service: Can attackers disrupt system availability?
- **E**levation of Privilege: Can attackers gain higher access levels?

This framework ensures no category of threat is overlooked, threats are identified systematically, not by intuition.


## What Was Identified & Mitigated

**Total Threats:** 48 · **Mitigation Rate:** 100%

**By Severity:**
- Critical/High: 8 threats (authentication, data tampering, DoS)
- Medium: 12 threats (information disclosure, repudiation)
- Low: 28 threats (edge cases, defense-in-depth)

**Top Threats:**

| Threat | Risk | Mitigation |
|---|---|---|
| Credential Theft | High | MFA, strong password policies, secure credential hashing |
| SQL Injection | High | Parameterized queries, input validation, least-privilege DB accounts |
| Unauthorized Access | High | RBAC, least privilege, API authentication tokens |
| Denial of Service | High | DDoS protection (Cloudflare), rate limiting, auto-scaling |


## Analysis Components

**Data Flow Diagram (DFD):** Mapped all system components and trust boundaries, where data moves, where it is stored, and where authentication happens.

**Attack Tree Example (Credential Theft):**

Goal: Steal user credentials
├─ Phishing email with fake login portal
├─ Man-in-the-middle attack on unencrypted traffic
├─ Brute force attack on weak passwords
├─ Database breach and credential extraction
└─ Social engineering support staff

**Risk Matrix:** Threats scored by Likelihood (1–5) × Impact (1–5). Scores of 15+ are critical; 6–14 are medium; 5 or below are low priority.


## Deliverables

- **PDF Threat Model Report:** Executive summary, threat catalog, risk matrix, mitigation roadmap
- **Threat Analysis Report (PDF):**  Interactive 48-threat breakdown with STRIDE categorization and severity scoring
- **PowerPoint Presentation:** Team findings, key risks, and mitigation strategies for stakeholder review


## Why This Matters in GRC

Threat modeling is foundational to GRC. In real organizations, threat models inform security architecture decisions, guide control selection, and provide auditors with evidence that risks were identified and addressed systematically — not guessed at. When new vulnerabilities emerge, the same framework can be reapplied.

The power of STRIDE is that it is disciplined. Auditors want to see methodology, not intuition. Your risk register needs to demonstrate that threats were identified systematically, and that every finding has a documented control.


## Tools & Methodologies

Microsoft Threat Modeling Tool · STRIDE Framework · Risk Scoring (Likelihood × Impact) · Attack Trees · Data Flow Diagrams · Risk Matrices


## Team Context

Completed collaboratively by a team of 8 cybersecurity interns as part of a group training exercise.
