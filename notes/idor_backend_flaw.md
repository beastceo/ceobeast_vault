# CONCEPTUAL CHECK #1 — THE IDOR BACKEND FLAW

**Date:** 2026-09-21
**Topic:** Insecure Direct Object Reference (IDOR) / Broken Object Level Authorization (BOLA)
**Context:** Juice Shop `/api/Users/{id}` endpoint

## The Question
You are logged in as User 25 (`ceobeast@moringa.com`). The server returned data for User 1 (admin) and User 2 (`jim@juice-sh.op`). What is the exact backend code flaw?

## The Godemode Answer
The developer wrote:
`SELECT * FROM Users WHERE id = [URL_PARAMETER]`

They **forgot** to append the session ownership check:
`AND id = [SESSION_USER_ID]`

## Why This Is Critical
The server **trusted client-side input** (the URL parameter) instead of enforcing **server-side ownership verification** using the authenticated JWT session.

This is a textbook **OWASP API1:2023 — Broken Object Level Authorization (BOLA)** vulnerability.

## The Impact
- An attacker can enumerate every User ID (`/api/Users/1`, `/api/Users/2`, `/api/Users/3`...)
- Harvest emails, roles, and PII from the entire user base
- Potentially escalate privileges by targeting admin accounts

## The Fix
The backend query must verify ownership:
`SELECT * FROM Users WHERE id = [URL_PARAMETER] AND id = [SESSION_USER_ID]`

Or use a middleware that validates object ownership on every request before returning any data.

## Real-World Relevance (HackerOne)
This exact vulnerability class is one of the highest-paying in Bug Bounty. BOLA/IDOR reports frequently pay $500 – $5,000+ depending on the scope and data sensitivity.
