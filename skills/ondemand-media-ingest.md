---
name: Ingest media for retrieval
description: Add a document/audio/video/image/YouTube source to OnDemand AI from a URL, list media, and delete it.
api: openapi/ondemand-openapi.json
operations: [createMediaURL, fetchMedia, deleteMedia]
---

# Ingest media into OnDemand AI

Use this to load source content into OnDemand AI so it can be retrieved by RAG chat queries.

## Auth
- Send your API key in the `apikey` HTTP header. Base URL: `https://api.on-demand.io`.

## Steps
1. **createMediaURL** — `POST /media/v1/public/file`. Create media from a URL of a document, audio, video, YouTube link, or image. Keep the returned `fileId`.
2. **fetchMedia** — `GET /media/v1/public/file`. List media with offset-based pagination (`page`, `limit`, `sort`) and filters (`plugins`, `externalUserId`, `source`).
3. **deleteMedia** — `DELETE /media/v1/public/file/{fileId}`. Remove a media file by its ID.

## Rules
- **Rate limits:** media upload is capped (5/min on the free plan); on HTTP 429 retry after the timeout.
- **Pagination:** media listing is offset-based (`page`/`limit`), unlike chat which is cursor-based.
- **Idempotency:** not supported — track `fileId`s you create to avoid duplicate ingestion.
- **Errors:** see errors/ondemand-problem-types.yml.
