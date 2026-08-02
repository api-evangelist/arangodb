---
name: Index a collection for search and vector similarity
description: >-
  Create persistent, inverted and vector indexes on an ArangoDB collection and
  wire an ArangoSearch view over them, so AQL full-text and nearest-neighbour
  queries are index-backed rather than full scans.
api: openapi/arangodb-core-openapi-original.json
generated: '2026-08-02'
method: generated
operations:
  - listIndexes
  - getIndex
  - createIndexPersistent
  - createIndexInverted
  - createIndexVector
  - deleteIndex
  - createAnalyzer
  - listAnalyzers
  - createView
  - getViewProperties
  - updateViewProperties
  - createViewSearchAlias
  - createAqlQueryCursor
---

# Index a collection for search and vector similarity

ArangoDB puts document, graph, full-text and vector retrieval behind one query
language. Which of them is fast depends entirely on the index you created.

## 1. See what already exists

- `listIndexes` — `GET /_db/{database-name}/_api/index?collection={name}`
- `getIndex` — `GET /_db/{database-name}/_api/index/{index-id}`

Every collection always has a primary index on `_key`, and edge collections have
an edge index on `_from`/`_to`. Everything else you create.

## 2. Persistent index — the workhorse

- `createIndexPersistent` — `POST /_db/{database-name}/_api/index#persistent`

```json
{ "type": "persistent", "fields": ["status", "createdAt"],
  "unique": false, "sparse": true, "inBackground": true }
```

Set `inBackground: true` on a populated collection so the build does not block
writes. `unique: true` gives you a uniqueness constraint — a violating insert
returns `409` / `errorNum 1210`.

Other single-purpose builders on the same path: `createIndexGeo`,
`createIndexTtl`, `createIndexMdi` (multi-dimensional), `createIndexFulltext`
(legacy — prefer inverted indexes plus a view).

## 3. Inverted index + ArangoSearch view — full-text and faceted search

Analyzers first, if the built-ins are not enough:

- `listAnalyzers` — `GET /_db/{database-name}/_api/analyzer`
- `createAnalyzer` — `POST /_db/{database-name}/_api/analyzer`
  (e.g. a `text` analyzer with a locale and stemming).

Then either of two shapes:

**a. `arangosearch` view over collection links**

- `createView` — `POST /_db/{database-name}/_api/view` with
  `{"name": "docsView", "type": "arangosearch", "links": {"docs": {"fields": {"body": {"analyzers": ["text_en"]}}}}}`
- `getViewProperties` / `updateViewProperties` to inspect and adjust the links.

**b. Inverted index + `search-alias` view** (the newer shape)

- `createIndexInverted` — `POST /_db/{database-name}/_api/index#inverted` on the
  collection, naming the analyzer per field.
- `createViewSearchAlias` — `POST /_db/{database-name}/_api/view#searchalias`
  referencing that index by name.

Query it through the cursor API with `SEARCH` and `BM25()`/`TFIDF()` scoring.

## 4. Vector index — nearest-neighbour retrieval

- `createIndexVector` — `POST /_db/{database-name}/_api/index#vector`

The field must hold a fixed-length numeric array (your embedding). Query it in
AQL with `APPROX_NEAR_COSINE()` / `APPROX_NEAR_L2()` and a `LIMIT`.

Operational notes from the release notes worth knowing:

- Vector index builds are asynchronous; a build can report a training error and
  clear it. `--vector-index-build-retry-backoff` (added 3.12.10) controls the
  retry backoff.
- If `APPROX_NEAR_*` fails to apply the index the query falls back to a scan —
  run `explainAqlQuery` and confirm the index appears in the plan.
- `arangodump` always includes vector indexes as of 3.12.10; `arangorestore`
  restores them **after** the data import.

## 5. Remove what you no longer need

- `deleteIndex` — `DELETE /_db/{database-name}/_api/index/{index-id}`

## Rules

- Always `explainAqlQuery` (`POST /_db/{database-name}/_api/explain`) before
  declaring a query indexed. The plan is the only proof.
- Build on a populated collection with `inBackground: true`.
- Index errors live in the 1200-range (`errorNum` 1210 unique constraint, 1212
  index not found) — see `errors/arangodb-error-codes.yml`.
