# GREENLIGHT VDP - MASTER ENGAGEMENT ROADMAP
**Date Started:** 2026-10-25
**Deadline:** 2026-11-04 (Bonus Window)
**Target:** https://hub.stage.greenlight.gl-tech.io/
**Operator:** beastceo

---

## PHASE 0: PRE-ENGAGEMENT & ENVIRONMENT (COMPLETED)
- [x] Target Selection (Greenlight VDP)
- [x] Scope Verification (Web Hub + APK/IPA)
- [x] Environment Cleanup (Disk space, Trash, Cache)
- [x] UFW Security Hardening
- [x] Support Ticket #194531 opened for Family Credentials
- [x] Target Acquisition (APK downloaded & hash verified)

## PHASE 1: INTELLIGENCE GATHERING (STATIC ANALYSIS)
- [ ] 1.1 Extract DEX files from APK
- [ ] 1.2 Extract all URLs from DEX files
- [ ] 1.3 Extract all API endpoints from DEX files
- [ ] 1.4 Grep for hardcoded secrets (API keys, AWS tokens, JWTs)
- [ ] 1.5 Analyze React Native JS Bundle (if applicable)
- [ ] 1.6 Document all findings in `recon/intel_log.md`

## PHASE 2: LIVE SURFACE MAPPING (UNAUTHENTICATED)
- [ ] 2.1 Initial HTTP Fingerprinting (Headers, Redirects)
- [ ] 2.2 Directory/Endpoint Fuzzing on Discovered API Paths
- [ ] 2.3 Analyze API responses for Information Disclosure
- [ ] 2.4 Test for Unauthenticated SSRF (if endpoints found)
- [ ] 2.5 Document all live endpoints in `recon/live_map.md`

## PHASE 3: VULNERABILITY HUNTING
- [ ] 3.1 Check for Missing Security Headers / Misconfigurations
- [ ] 3.2 Test unauthenticated endpoints for IDOR/BOLA
- [ ] 3.3 Analyze error messages for stack traces or debug info
- [ ] 3.4 [BLOCKED] Awaiting credentials for authenticated testing
- [ ] 3.5 Test for Client-Side Vulnerabilities (DOM XSS, Open Redirect)

## PHASE 4: EXPLOITATION & EVIDENCE (ZERO ACCIDENT)
- [ ] 4.1 Reproduce finding independently (Min. 2 times)
- [ ] 4.2 Capture raw HTTP request/response
- [ ] 4.3 Create Proof of Concept (Screenshot/Video/Curl)
- [ ] 4.4 Verify finding does not violate Safe Harbor or DoS rules
- [ ] 4.5 Package evidence in `evidence/` folder

## PHASE 5: REPORTING & SUBMISSION (8.5/10 STANDARD)
- [ ] 5.1 Draft report using CEOBEAST Template
- [ ] 5.2 Include Transparency Note (Missing Credentials, Ticket #194531)
- [ ] 5.3 White Team Review (Scope, PII, Severity)
- [ ] 5.4 Submit to Bugcrowd
- [ ] 5.5 Log Submission ID in vault
