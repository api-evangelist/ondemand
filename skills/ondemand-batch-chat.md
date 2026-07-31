---
name: Submit and retrieve a chat batch
description: Submit a batch of chat queries for asynchronous processing on OnDemand AI, then retrieve or delete the batch.
api: openapi/ondemand-openapi.json
operations: [createChatBatch, getChatBatch, deleteChatBatch]
---

# Run an asynchronous chat batch on OnDemand AI

Use this to process many chat queries asynchronously instead of one at a time.

## Auth
- Send your API key in the `apikey` HTTP header. Base URL: `https://api.on-demand.io`.

## Steps
1. **createChatBatch** — `POST /chat/v1/batches`. Submit a batch of chat queries for asynchronous processing; keep the returned `batchId`.
2. **getChatBatch** — `GET /chat/v1/batches/{batchId}`. Poll the batch to check status and collect results. (Use `GET /chat/v1/batches` to list batches with pagination.)
3. **deleteChatBatch** — `DELETE /chat/v1/batches/{batchId}`. Delete a batch when finished.

## Rules
- **Async model:** results are not returned inline — poll `getChatBatch`, or configure a webhook (asyncapi/ondemand-webhooks.yml).
- **Rate limits:** batch queries count against the RAG call limit; on HTTP 429 back off.
- **Idempotency:** not supported — persist `batchId` to avoid resubmitting the same batch.
- **Errors:** see errors/ondemand-problem-types.yml.
