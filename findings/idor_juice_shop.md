# VULNERABILITY REPORT: IDOR in User Profile

**Title:** IDOR in User Profile - Unauthorized Access to Other Users' Data

**Summary:** 
The application fails to verify object ownership at the API level. An authenticated user can access the private data of any other user by simply changing the User ID in the URL parameter.

**Description:**
The endpoint `/api/Users/{id}` returns sensitive user data (email, role, creation date) without validating whether the requesting user's session is authorized to view that specific object. The server trusts the client-supplied `id` parameter.

**Steps to Reproduce:**
1. Register a normal user account (e.g., `ceobeast@moringa.com`). Note the User ID (e.g., 25).
2. Login to obtain a valid JWT token.
3. Send the following request: `GET /api/Users/1` with the `Authorization: Bearer <YOUR_JWT_TOKEN>` header. Note the successful extraction of Admin data.
4. Change the ID to `2`: `GET /api/Users/2` with the same token. Note the successful extraction of User 2 data (`jim@juice-sh.op`).

**Expected Result:**
The server should return a `403 Forbidden` or `401 Unauthorized` error. A user should only access their own profile (ID 25).

**Actual Result:**
The server returns `200 OK` with the sensitive data of User 1 (Admin) and User 2 (Customer).

**Evidence:**
[Paste your exact terminal output showing the successful IDOR requests for User 1 and User 2 here]

**Impact & Severity:**
- **Impact:** High. An attacker can enumerate all user IDs to harvest emails, roles (including admin), and other sensitive data, leading to mass data exfiltration and potential privilege escalation.
- **Severity:** High (CVSS 7.5 - High).

**Recommendation:**
Implement strict server-side authorization checks. The backend query must verify ownership:
`SELECT * FROM Users WHERE id = [URL_PARAMETER] AND id = [SESSION_USER_ID]`
Alternatively, use a middleware that validates object ownership on every request before returning any data.
