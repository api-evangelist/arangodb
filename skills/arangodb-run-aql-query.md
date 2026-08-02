---
name: Run an AQL query against ArangoDB and page the whole result set
description: >-
  Authenticate to an ArangoDB deployment, submit an AQL query with bind
  variables, and drain every batch of the server-side cursor without leaking
  cursors or reading stale data.
api: openapi/arangodb-core-openapi-original.json
generated: '2026-08-02'
method: generated
operations:
  - createSessionToken
  - createAqlQueryCursor
  - getNextAqlQueryCursorBatchPut
  - deleteAqlQueryCursor
  - parseAqlQuery
  - explainAqlQuery
---

# Run an AQL query and page the result set

Every read in ArangoDB that is more than a single-document lookup goes through
AQL and the cursor API. There is no offset/limit convention on the REST
resources — batching is a property of the cursor.

## 1. Authenticate

Either send HTTP Basic on every request, or exchange credentials once for a JWT:

- `createSessionToken` — `POST /_open/auth` with `{"username": …, "password": …}`
  returns `{"jwt": "…"}`. Send it as `Authorization: Bearer <jwt>` afterwards.

Tokens expire; the bounds are set per deployment with
`--auth.minimal-jwt-expiry-time` / `--auth.maximal-jwt-expiry-time`. On a `401`
re-issue the token rather than retrying the same call.

## 2. Validate the query before you run it (optional but cheap)

- `parseAqlQuery` — `POST /_db/{database-name}/_api/query` returns the parse
  result and the bind parameter names, so you can confirm the query is
  syntactically valid and that you are supplying every `@variable`.
- `explainAqlQuery` — `POST /_db/{database-name}/_api/explain` returns the
  execution plan. Use it to confirm an index is being used before running an
  expensive traversal.

## 3. Open the cursor

- `createAqlQueryCursor` — `POST /_db/{database-name}/_api/cursor`

Body fields that matter:

- `query` — the AQL string. **Never interpolate user input into it.**
- `bindVars` — every value goes here, as `@name`. Collection names go in as
  `@@name`.
- `batchSize` — documents per batch. Default is server-configured; set it
  explicitly for predictable memory use.
- `count` — set `true` only if you need the total; it costs a full count.
- `ttl` — seconds the server keeps the cursor alive between batches.
- `memoryLimit` — hard cap; the query fails rather than exhausting the server.
- `options.stream` — `true` to stream results lazily instead of materialising.

Useful headers:

- `x-arango-allow-dirty-read: true` — in a cluster, permit reads from followers.
  Only set this when a slightly stale read is acceptable.
- `x-arango-trx-id: <id>` — run the query inside an open stream transaction.

The response is `{"result": [...], "hasMore": true|false, "id": "<cursor-id>",
"count": n, "cached": bool, "extra": {"stats": {...}}}`.

## 4. Drain the cursor

While `hasMore` is `true`:

- `getNextAqlQueryCursorBatchPut` — `PUT /_db/{database-name}/_api/cursor/{cursor-identifier}`

Each call returns the next batch and an updated `hasMore`. Keep calling until
`hasMore` is `false`. There is no page-number or offset parameter — the cursor id
is the only pagination state.

## 5. Release the cursor

- `deleteAqlQueryCursor` — `DELETE /_db/{database-name}/_api/cursor/{cursor-identifier}`

Call this if you stop early. A cursor you abandon holds server memory until its
`ttl` expires.

## Rules

- **Bind variables, always.** `bindVars` is the only injection-safe way to pass
  values, and it lets the server reuse the query plan.
- **No idempotency keys exist.** Opening a cursor is a `POST`; a retried
  `createAqlQueryCursor` for a *read* query is harmless, but a retried
  `POST /_api/cursor` running a mutating AQL statement (`INSERT`/`UPDATE`/
  `REMOVE`) will apply twice. See `conventions/arangodb-conventions.yml`.
- **Read errors from `errorNum`, never the message.** A failure returns
  `{"error": true, "code": <http-status>, "errorNum": <n>, "errorMessage": "…"}`.
  Branch on `errorNum`; the text can change for the same error kind. The full
  348-entry registry is in `errors/arangodb-error-codes.yml`.
- **`503` means the request queue is full**, not that the query failed. Back off
  and retry.
