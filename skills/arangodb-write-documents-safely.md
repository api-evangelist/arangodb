---
name: Create and update ArangoDB documents safely
description: >-
  Insert, update and delete documents in an ArangoDB collection using explicit
  keys and revision preconditions so a retried request cannot duplicate a write
  or clobber a newer one — the substitute for the Idempotency-Key header
  ArangoDB does not publish.
api: openapi/arangodb-core-openapi-original.json
generated: '2026-08-02'
method: generated
operations:
  - createCollection
  - createDocument
  - createDocuments
  - getDocument
  - getDocumentHeader
  - updateDocument
  - replaceDocument
  - deleteDocument
  - beginStreamTransaction
  - commitStreamTransaction
  - abortStreamTransaction
---

# Create and update documents safely

ArangoDB has **no `Idempotency-Key` header**. Safe retry is built out of two
things the API does give you: client-assigned `_key`s and revision
preconditions on `_rev`.

## 1. Make sure the collection exists

- `listCollections` — `GET /_db/{database-name}/_api/collection`
- `createCollection` — `POST /_db/{database-name}/_api/collection` with
  `{"name": …, "type": 2}` for a document collection or `"type": 3` for an edge
  collection.

Creating a collection that already exists returns `409` /
`errorNum 1207` (duplicate name). Treat that as success.

## 2. Insert with an explicit `_key`

- `createDocument` — `POST /_db/{database-name}/_api/document/{collection}`
- `createDocuments` — `POST /_db/{database-name}/_api/document/{collection}#multiple`
  for an array body.

Put a deterministic, caller-derived `_key` in the body. Then a retry of the same
insert fails with `409` / `errorNum 1210` (unique constraint violated) instead
of creating a second document — that is the idempotency substitute.

Useful query parameters:

- `waitForSync` — force the write to disk before responding.
- `returnNew` / `returnOld` — get the full document back instead of just the meta.
- `overwriteMode` — `ignore`, `replace`, `update` or `conflict`. `ignore` turns a
  duplicate insert into a no-op success, which is the closest thing to an
  upsert-safe retry.
- `silent` — suppress the per-document response envelope on bulk writes.

The response carries `_id`, `_key` and `_rev`. **Keep the `_rev`.**

## 3. Update or replace with a precondition

- `updateDocument` — `PATCH /_db/{database-name}/_api/document/{collection}/{key}`
  (merge semantics)
- `replaceDocument` — `PUT /…/{key}` (full replacement)

Send the `_rev` you last saw as the `If-Match` header. If someone else wrote in
between, you get `412` (precondition failed) / `errorNum 1200` (conflict) and
your stale write is rejected instead of silently winning.

Relevant parameters: `keepNull`, `mergeObjects`, `ignoreRevs`,
`refillIndexCaches`, `versionAttribute`.

**Do not set `ignoreRevs: true` on a retry path** — it is exactly the flag that
turns a safe conditional write back into a last-writer-wins race.

## 4. Delete with a precondition

- `deleteDocument` — `DELETE /_db/{database-name}/_api/document/{collection}/{key}`
  with `If-Match: <rev>`.

A delete of an already-deleted document returns `404` / `errorNum 1202`
(document not found). For an idempotent delete, treat `404` as success.

## 5. Group writes that must land together

- `beginStreamTransaction` — `POST /_db/{database-name}/_api/transaction/begin`,
  declaring `collections.read` / `.write` / `.exclusive`.
- Send every subsequent write with the `x-arango-trx-id` header set to the
  returned transaction id.
- `commitStreamTransaction` — `PUT /_db/{database-name}/_api/transaction/{transaction-id}`
- `abortStreamTransaction` — `DELETE /…/{transaction-id}`

Always abort on failure. An abandoned transaction holds locks.

## Rules

- **Never retry a keyless `POST`.** Without `_key` the server assigns one and a
  retry creates a duplicate.
- **Cheap existence check:** `getDocumentHeader` (`HEAD`) returns the `Etag`
  (the `_rev`) without transferring the body; `getDocument` (`GET`) supports
  `If-None-Match` for a `304`.
- **Branch on `errorNum`, not on the message text** — registry in
  `errors/arangodb-error-codes.yml`.
- Cross-cutting semantics: `conventions/arangodb-conventions.yml`.
