# Configuration

For basic configuration instructions, see [this documentation](https://developers.openai.com/codex/config-basic).

For advanced configuration instructions, see [this documentation](https://developers.openai.com/codex/config-advanced).

For a full configuration reference, see [this documentation](https://developers.openai.com/codex/config-reference).

## HTTP request retries

Configure HTTP request retries for a custom provider in `config.toml`:

```toml
model_provider = "openai-custom"

[model_providers.openai-custom]
name = "OpenAI with HTTP retries"
base_url = "https://api.openai.com/v1"
env_key = "OPENAI_API_KEY"
request_max_retries = 8
retry_429 = true
request_retry_delay_ms = 1000
```

`retry_429` enables retries for HTTP 429 (Too Many Requests) responses and
defaults to `false`. `request_max_retries` limits the number of retries after
the initial request (default: 4, maximum: 100); setting it to `0` disables
HTTP request retries, including 429 retries.

`request_retry_delay_ms` sets the base delay for HTTP request retries in
milliseconds (default: 200). The delay doubles for each subsequent retry and
includes jitter of up to 10%. A valid server `Retry-After` header takes
precedence over this delay. These settings apply to HTTP request retries;
`stream_max_retries` controls stream reconnection attempts separately.

## Lifecycle hooks

Admins can set top-level `allow_managed_hooks_only = true` in
`requirements.toml` to ignore user, project, and session hook configs while
still allowing managed hooks from requirements and managed config layers. This
setting is only supported in `requirements.toml`; putting it in `config.toml`
does not enable managed-hooks-only mode.
