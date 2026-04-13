---
title: LTX Video API Error Reference
type: reference
created: 2026-04-13
updated: 2026-04-13
sources:
  - https://docs.ltx.video/errors
  - https://docs.ltx.video/debugging
  - https://docs.ltx.video/rate-limits
tags:
  - api
  - errors
  - debugging
  - ltx-video
---

# LTX Video API Error Reference

All errors from the [[ltx-video-api]] follow a consistent JSON structure. This page covers every documented error type and debugging guidance.

## Error Response Structure

```json
{
  "type": "error",
  "error": {
    "type": "error_type",
    "message": "Human-readable error message"
  }
}
```

## Error Types

| HTTP Status | Error Type | Description |
|-------------|-----------|-------------|
| 400 | `invalid_request_error` | Invalid request parameters or validation errors |
| 401 | `authentication_error` | Missing or invalid API key |
| 402 | `insufficient_funds_error` | Account lacks sufficient credits |
| 413 | `request_too_large` | Payload exceeds maximum byte limit |
| 422 | `content_filtered_error` | Content rejected by safety filters |
| 429 | `rate_limit_error` | Request rate limit exceeded |
| 429 | `concurrency_limit_error` | Too many concurrent requests |
| 500 | `api_error` | Unexpected server-side failure |
| 503 | `service_unavailable` | Service temporarily down |
| 504 | (timeout) | Request timed out |

Note: HTTP 429 may indicate either a rate limit or concurrency limit -- check the `error.type` field to distinguish.

## Debugging Guide

### 400 -- Invalid Request
- Verify all required parameters are present.
- Check parameter types match the schema.
- Ensure resolution format is correct (e.g., `1920x1080`).
- Confirm duration is within model limits.

### 401 -- Authentication Error
- Verify the API key is correct.
- Confirm `Authorization` header format: `Bearer YOUR_API_KEY`.
- Regenerate the key at https://console.ltx.video if needed.

### 402 -- Insufficient Funds
- Check account balance in the developer console dashboard.
- Add credits before retrying.
- Consider shorter videos or lower resolution to reduce cost.

### 413 -- Payload Too Large
- Consult [[ltx-video-api-input-formats]] for size limits per upload method.
- Use the upload endpoint for larger files (up to 100 MB).
- Compress images or videos before sending.

### 422 -- Content Filtered
- Adjust request content to comply with platform safety policies.
- Modify prompts to avoid triggering content filters.
- Modify input images or videos if they triggered the filter.

### 429 -- Rate / Concurrency Limit
- Check the `Retry-After` header for wait time in seconds.
- Reduce concurrent request count.
- Implement exponential backoff.
- Contact support for limit increases.

### 500 -- Server Error
- Retry after a brief delay.
- If persistent, contact support.

### 503 -- Service Unavailable
- The service is temporarily down for maintenance.
- Retry after a delay.

### 504 -- Timeout
- The request took too long to process.
- Try with shorter duration or lower resolution.
- Retry the request.

## Related Pages

- [[ltx-video-api]]
- [[ltx-video-api-endpoints]]
- [[ltx-video-api-input-formats]]
- [[ltx-video-api-pricing]]
