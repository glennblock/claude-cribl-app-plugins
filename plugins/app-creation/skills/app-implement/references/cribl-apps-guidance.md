---
name: cribl-apps-guidance
description: Dos and don'ts for implementing Cribl Apps to avoid common pitfalls
last_updated: 2026-07-31
---

# Cribl Apps Implementation Guidance

Reference for developers implementing Cribl Apps. Keep this updated as we discover patterns and pitfalls.

## General

## Permissions

### ✅ DO
- If the app needs access to user role or team membership, Copy `/references/policies.yml` to the apps `/config`.
- Declare all Cribl endpoints the app uses in `/config/policies.yml`. 

## KV Store

### ✅ DO
- Use kv for caching transient data (selections, filters etc)
- Use kv for storing large documents that the app depends on. For example a notebook app could store notebooks in kv. Or a Splunk migrator app might store Splunk configs. 
- Store user-specific state with the userid in the key. See agents.md to see how to access the key.

### ❌ DON'T
- Don't EVER use browser local storage. Only use kv for any kind of persistence.
- Don't use KV store as primary data source (it's cache, not database)
- Don't rely on KV for strict consistency or transaction semantics
- Don't store JSON objects, they must be serialized / deserialized as strings.

---

## Error Handling

### ✅ DO
- Return `null` or empty result on transient errors (retry-able)
- Log errors with context (what failed, why, inputs if safe)
- Validate external API responses before consuming

### ❌ DON'T
- Don't silently swallow errors (log them)
- Don't throw generic `Error` objects; use descriptive messages
- Don't expose stack traces to end users

---

## Performance

### ✅ DO
- Cache expensive calculations (API calls, parsing, regex)
- Use timeouts on external service calls
- Test with large payloads and high throughput

### ❌ DON'T
- Don't synchronous loop over large datasets
- Don't create regex patterns in hot paths (compile once)
- Don't make unbounded network requests

---

## Debugging

### ✅ DO
- Inject console debugging as needed to verify the format of API response payloads.

---

## Testing

### ✅ DO
- Write unit tests for parsing/transformation logic
- Test edge cases (empty input, malformed data, null values)
- Mock external APIs

### ❌ DON'T
- Don't skip tests because "it's simple"
- Don't test only the happy path

---

## Configuration & Secrets

### ✅ DO
- Use `defaults` in manifest for defaults
- Accept secrets via environment variables or instance config
- Document required config fields in the README or manifest comments
- Store secrets in kv with the ?encrypted=true param in the url. 
- Create a sentinel key for encrypted values `myapp:has_mysecret` to allow the app to detect if `myapp:mysecret` was set.

### ❌ DON'T
- Don't hardcode API keys or credentials
- Don't assume config is present (validate on init)
- Don't store secrets in KV store unencrypted

---

## API Integration

### ✅ DO
- Implement retry logic with exponential backoff
- Respect rate limits (check headers, honor X-RateLimit-Reset)
- Use reasonable connection/read timeouts

### ❌ DON'T
- Don't retry indefinitely
- Don't ignore rate limit headers
- Don't allow timeouts to be unbounded

---

## Manifest & Structure

### ✅ DO
- Keep `manifest.json` clean and up-to-date
- Use descriptive names for functions and event handlers
- Document inputs/outputs in comments

### ❌ DON'T
- Don't leave dead code or unused functions
- Don't forget to update manifest when adding functions
- Don't make manifest overly complex

---

## Dependencies

### ✅ DO
- Keep dependencies minimal
- Pin versions in `package-lock.json`
- Audit for security vulnerabilities (`npm audit`)

### ❌ DON'T
- Don't add heavy dependencies for simple tasks (lodash for 1 function, etc.)
- Don't use unstable/unvetted packages
- Don't ignore security warnings

