---
name: Stream a chat completion from Featherless
description: >-
  Generate a streamed (server-sent-events) chat completion from a Featherless AI
  open-weight model for low-latency, token-by-token output.
api: openapi/recursal-ai-inc-featherless-openapi.yml
operations:
- createChatCompletion
---

# Stream a chat completion from Featherless

Featherless AI is OpenAI-compatible, so streaming works exactly like the OpenAI
SDK with `base_url` set to `https://api.featherless.ai/v1`.

## Auth
`Authorization: Bearer FEATHERLESS_API_KEY`.

## Steps
1. Call `createChatCompletion` (`POST /v1/chat/completions`) with `model`,
   `messages`, and `stream: true`.
2. Read the `text/event-stream` response and concatenate the delta content from
   each chunk until the stream closes.

## Rules
- Streaming does not change error handling: a **429** still applies per the
  plan's concurrent-unit budget (`rate-limits/recursal-ai-inc-rate-limits.yml`).
- Inference requests are **not idempotent** — there is no idempotency-key header;
  do not blindly retry a partially streamed response. See
  `conventions/recursal-ai-inc-conventions.yml`.
- Error catalog: `errors/recursal-ai-inc-problem-types.yml`.
