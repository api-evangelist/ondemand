---
name: Run a RAG chat query
description: Create a chat session on OnDemand AI, submit a RAG query against a fulfillment model, and read back the answer.
api: openapi/ondemand-openapi.json
operations: [createChatSession, submitQuery, getChatMessages]
---

# Run a RAG chat query on OnDemand AI

Use this to ask a question through OnDemand AI's RAG platform and get an answer from a fulfillment model (predefined, BYOI, or BYOM).

## Auth
- Send your API key in the `apikey` HTTP header on every request. Base URL: `https://api.on-demand.io`.
- Get a key from Settings > API Key Management (see docs/authentication). The key is shown once — store it securely.

## Steps
1. **createChatSession** — `POST /chat/v1/sessions`. Create a session; keep the returned `sessionId`.
2. **submitQuery** — `POST /chat/v1/sessions/{sessionId}/query`. Send `query`, `endpointId` (the fulfillment model, e.g. `predefined-openai-gpt4o`), and `responseMode` (`sync`, `stream`, or `webhook`). Optionally pass `pluginIds` (max 20) to give the agent access to knowledge/REST/IoT agents.
3. **getChatMessages** — `GET /chat/v1/sessions/{sessionId}/messages`. Retrieve the messages; supports cursor-based pagination (`cursor`, `limit`, `sort`).

## Rules
- **Response modes:** use `sync` to get the answer inline; `stream` for token streaming; `webhook` to have the answer delivered to your configured webhook endpoint (see asyncapi/ondemand-webhooks.yml).
- **Pagination:** message/session lists are cursor-based; pass `cursor` from the previous page.
- **Rate limits:** RAG calls are capped (100/min on the free plan); on HTTP 429 back off and retry after the timeout.
- **Idempotency:** not supported — do not blindly retry a POST that may have succeeded.
- **Errors:** 401 = bad/missing `apikey`; 403 = not in plan; 400 = malformed body (see errors/ondemand-problem-types.yml).
