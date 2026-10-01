# Systemic Arbitrary Local File Read & Credential Injection in Redis Insight

## Summary
Redis Insight 3.8.0 reads arbitrary files from the filesystem when importing TLS/SSH credentials. The contents are stored as trusted credentials and used to establish outbound connections to attacker-controlled servers. This results in a systemic Local File Read (LFR) and credential injection vulnerability across four import paths.

## Vulnerability Type
- Arbitrary Local File Read (CWE-22 / CWE-200)
- Credential Injection (CWE-522)
- Insufficient Input Validation (CWE-20)

## Severity
**High (CVSS 4.0: ~7.5)**

## Affected Component
- Product: Redis Insight 3.8.0
- Installation: Docker (`redis/redisinsight:latest`)
- Endpoint: `POST /api/databases/import`
- Vulnerable Parameters: `sshOptions.privateKey`, `tlsCaCert`, `tlsClientCert`, `tlsClientKey`

## Root Cause
The function `getPemBodyFromFileSync(path)` in `dist/main/main.js` (line 166410) calls `fs.readFileSync(path)` without path validation:

```javascript
const getPemBodyFromFileSync = (path) => (0, fs_1.readFileSync)(path).toString('utf8');


## Second Independent Reproduction (2026-10-01 14:16)

To confirm reproducibility, the attack chain was re-run in a fresh environment:

- **New working directory:** `/tmp/ri_test`
- **Fresh TLS materials:** new CA, client cert, client key
- **New Docker container:** `redisinsight_test`
- **Same import payload structure**
- **Result:** `{"total":1,"success":[{"index":0,"status":"success","host":"172.17.0.1","port":6380}],"partial":[],"fail":[]}`
- **Database created:** `488345d0-8c5f-418f-9404-961fe5da9ad8`

This reproduction confirms the vulnerability is inherent to Redis Insight 3.8.0 and not environment-specific.
