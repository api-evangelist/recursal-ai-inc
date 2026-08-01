---
name: Chat with an open-weight model on Featherless
description: >-
  Pick an available open-weight model and generate a chat completion against
  the Featherless AI OpenAI-compatible inference API.
api: openapi/recursal-ai-inc-featherless-openapi.yml
operations:
- listModels
- createChatCompletion
---

# Chat with an open-weight model on Featherless

Featherless AI (operated by Recursal AI) is an OpenAI-compatible serverless
inference platform. Base URL: `https://api.featherless.ai/v1`.

## Auth
Send an HTTP Bearer token: `Authorization: Bearer FEATHERLESS_API_KEY`. Create a
key in account settings. See `authentication/recursal-ai-inc-authentication.yml`.

## Steps
1. **Discover a model** — call `listModels` (`GET /v1/models`) and choose a model
   `id` (a Hugging Face id such as `Qwen/Qwen2.5-7B-Instruct`).
2. **Generate** — call `createChatCompletion` (`POST /v1/chat/completions`) with
   `model` and a `messages` array of `{role, content}` objects. Read the reply
   from `choices[0].message.content`.
3. Optionally set `HTTP-Referer` and `X-Title` headers to identify your app for
   support attribution.

## Rules
- If the model is gated you get **403** — accept the model license via its model
  page first.
- A **400** means the model is cold (not loaded to GPU); retry after warm-up
  (5 min to 1 hour by size).
- A **429** means you exceeded your plan's reserved concurrent units; reduce
  in-flight requests or upgrade. See `rate-limits/recursal-ai-inc-rate-limits.yml`.
- Full error semantics: `errors/recursal-ai-inc-problem-types.yml`.
