# Missing Rate Limiting Evidence

**Date:** 2026-10-25
**Target:** https://onboarding.stage.greenlight.gl-tech.io/onboarding/phone-number
**Vulnerability:** Missing Rate Limiting on SMS OTP Generation

## Reproduction Steps
1. Captured the `POST` request to `/onboarding/phone-number`.
2. Executed a bash loop to send 9 requests in under 5 seconds to different dummy phone numbers.
3. The server returned `HTTP 204` for all 9 requests, indicating successful OTP generation.

## Raw Output
Request 1: 204
Request 2: 204
Request 3: 204
Request 4: 204
Request 5: 204
Request 6: 204
Request 7: 204
Request 8: 204
Request 9: 204
Request 10: 400 (Note: Failed due to invalid phone number format on the 10th iteration)

## Impact
- An attacker can automate requests to this endpoint to spam victims with SMS messages.
- This can lead to financial loss for Greenlight, as SMS gateways charge per message.
