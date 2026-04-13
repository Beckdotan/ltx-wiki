# LTX Video API Error Reference

**Source:** https://docs.ltx.video/errors
**Fetched:** 2026-04-13

---

## Error Response Structure

All API errors follow this JSON format:
```json
{
  "type": "error",
  "error": {
    "type": "error_type",
    "message": "Human-readable error message"
  }
}
```

---

## Error Types by HTTP Status

| HTTP Status | Error Type | Description |
|-------------|-----------|-------------|
| 400 | `invalid_request_error` | Invalid request parameters or validation errors |
| 401 | `authentication_error` | Missing or invalid API key |
| 402 | `insufficient_funds_error` | Account lacks sufficient credits for the request |
| 413 | `request_too_large` | Payload exceeds maximum byte limit |
| 422 | `content_filtered_error` | Content rejected by safety filters |
| 429 | `rate_limit_error` | Exceeds request rate limits |
| 429 | `concurrency_limit_error` | Concurrent request limit breached |
| 500 | `api_error` | Unexpected server-side failure |
| 503 | `service_unavailable` | Service temporarily down |
| 504 | (timeout) | Request timeout |

---

## Debugging Guide

### 400 - Invalid Request
- Check all required parameters are present
- Verify parameter types match the schema
- Ensure resolution format is correct (e.g., "1920x1080")
- Check duration is within model limits

### 401 - Authentication Error
- Verify API key is correct
- Check Authorization header format: `Bearer YOUR_API_KEY`
- Regenerate key at https://console.ltx.video if needed

### 402 - Insufficient Funds
- Check account balance in the developer console dashboard
- Add credits before retrying
- Consider shorter videos or lower resolution to reduce costs

### 413 - Payload Too Large
- Consult Input Formats documentation for size limits
- Use the Upload endpoint for larger files
- Compress images/videos before sending

### 422 - Content Filtered
- Adjust request content to comply with platform safety policies
- Modify prompts to avoid triggering content filters
- Modify input images/videos if they triggered the filter

### 429 - Rate/Concurrency Limit
- Check `Retry-After` header for wait time
- Reduce concurrent request count
- Implement exponential backoff
- Contact support for limit increases

### 500 - Server Error
- Retry the request after a brief delay
- If persistent, contact support

### 503 - Service Unavailable
- Service is temporarily down for maintenance
- Retry after a delay

### 504 - Timeout
- The request took too long to process
- Try with shorter duration or lower resolution
- Retry the request

---

## Related Links

- **Error Reference:** https://docs.ltx.video/errors
- **Debugging Guide:** https://docs.ltx.video/debugging
- **Rate Limits:** https://docs.ltx.video/rate-limits
- **Input Formats:** https://docs.ltx.video/input-formats
