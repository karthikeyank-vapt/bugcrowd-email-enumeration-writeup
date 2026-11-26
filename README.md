<div align="center">

# Cross-Account Email Verification via Insufficient Authorization
### Guest Confirmation Endpoint | Bug Bounty Write-Up

![Platform](https://img.shields.io/badge/Platform-Bugcrowd-F26822?style=for-the-badge)
![Priority](https://img.shields.io/badge/Priority-P4-yellow?style=for-the-badge)
![Category](https://img.shields.io/badge/VRT-Broken%20Access%20Control-red?style=for-the-badge)
![Type](https://img.shields.io/badge/Type-Email%20Enumeration-blue?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Accepted-brightgreen?style=for-the-badge)

**Researcher:** Karthikeyan K &nbsp;|&nbsp; **Role:** Penetration Tester (VAPT / Web Application Security)

</div>

---

## Overview

This repository documents a security finding I reported through a **Bugcrowd** bug bounty program. A guest confirmation endpoint did not sufficiently enforce authorization, which allowed cross-account email verification behaviour and led to **email enumeration**.

The finding was initially reported as **Broken Access Control > IDOR**. During triage it was **reclassified as Username/Email Enumeration (Non-Brute Force)** and rated **P4**. The report was **accepted**.

> **Responsible disclosure notice:** The target program, domain, endpoints, parameters, request/response data and any account details are intentionally **withheld**. This write-up focuses on the methodology and lessons learned, not on reproduction steps.

---

## Summary

| Field | Details |
|---|---|
| **Platform** | Bugcrowd |
| **Report title** | Cross-Account Email Verification via Insufficient Authorization on Guest Confirmation Endpoint |
| **Vulnerability class** | Broken Access Control (BAC) |
| **Initial VRT** | BAC > Insecure Direct Object References (IDOR) |
| **Final VRT** | BAC > Username/Email Enumeration > Non-Brute Force |
| **Priority** | None to **P4** |
| **Outcome** | Accepted (P4) |
| **Impact** | Email enumeration |

---

## Timeline

| Stage | Event |
|---|---|
| 1 | Identified unexpected behaviour in the guest confirmation flow during authorized testing |
| 2 | Validated the authorization gap and confirmed it was repeatable |
| 3 | Submitted a report with clear impact reasoning and evidence |
| 4 | Triage reclassified the issue from IDOR to Email Enumeration (Non-Brute Force) |
| 5 | Priority set to **P4** |
| 6 | Report accepted |

---

## Technical Summary

A guest-facing confirmation endpoint accepted requests without sufficiently verifying that the requester was authorized to act on the email address or account context involved. As a result, the application's behaviour could be used to learn information about other accounts, which is the basis of **email enumeration**.

**Root cause (high level):** insufficient server-side authorization checks on a guest (unauthenticated) flow.

**Why it matters:**
- Confirms whether a given email address is associated with an account
- Can support targeted phishing and credential-stuffing preparation
- Weakens user privacy, since account existence should not be disclosed to unauthorized parties

---

## Evidence

Screenshots below are redacted (program name, usernames, report IDs, domains and emails removed).

| Triage reclassification (VRT change, priority P4) |
|---|
| ![Triage update](screenshots/triage-update.png) |

---

## Triage Outcome

- **VRT changed:** Broken Access Control > IDOR → Broken Access Control > Username/Email Enumeration > Non-Brute Force
- **Priority:** none → **P4**
- **Result:** accepted

The reclassification reflects how triage rates what is **demonstrated**: proven impact, rather than the bug class the researcher believes they found.

---

## Lessons Learned

1. **Triage rates proven impact, not the bug class.** An IDOR hypothesis becomes email enumeration when the demonstrated outcome is account-existence disclosure. Frame the report around impact you can prove.
2. **Reclassification is not rejection.** A lower-severity rating still means the issue was valid. Understanding why triage re-rated it makes the next report stronger.
3. **Guest flows deserve the same authorization scrutiny as authenticated ones.** Unauthenticated endpoints are easy to overlook during design and review.
4. **Write the impact section first.** Strong impact reasoning up front reduces back-and-forth with triage.

---

## Recommended Remediation (General)

- Enforce server-side authorization on every guest confirmation action
- Return **uniform responses** regardless of whether an email is registered
- Apply **rate limiting** and abuse monitoring on confirmation endpoints
- Avoid exposing account state through error messages, timing or response differences

---

## Skills Demonstrated

`Broken Access Control` &nbsp; `IDOR Testing` &nbsp; `Email Enumeration` &nbsp; `Authorization Testing` &nbsp; `Burp Suite` &nbsp; `Bug Bounty Reporting` &nbsp; `VRT / CVSS Reasoning` &nbsp; `Responsible Disclosure`

---

## Disclaimer

This work was performed within the scope and rules of an authorized bug bounty program. No sensitive data was accessed, stored or shared. Details are withheld to respect program confidentiality and responsible disclosure. This repository is for educational and portfolio purposes only.

---

<div align="center">

**Karthikeyan K** | Penetration Tester | VAPT | Web Application Security

[LinkedIn](https://www.linkedin.com/in/karthikeyan-k-1b59b7327) &nbsp;|&nbsp; [GitHub](https://github.com/karthikeyank-vapt) &nbsp;|&nbsp; [Portfolio](https://karthikeyank-vapt.github.io/karthikeyank-portfolio/)

</div>
