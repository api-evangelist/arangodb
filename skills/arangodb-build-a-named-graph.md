---
name: Build a named graph and traverse it
description: >-
  Stand up an ArangoDB named graph — vertex collections, edge collections and
  edge definitions — load vertices and edges, then traverse it with AQL through
  the cursor API.
api: openapi/arangodb-core-openapi-original.json
generated: '2026-08-02'
method: generated
operations:
  - createCollection
  - createGraph
  - listGraphs
  - getGraph
  - listVertexCollections
  - addVertexCollection
  - listEdgeCollections
  - createDocuments
  - getVertexEdges
  - createAqlQueryCursor
  - deleteGraph
---

# Build a named graph and traverse it

ArangoDB is graph-native: a graph is a declared set of **edge definitions** over
ordinary collections. Vertices are documents; edges are documents in an edge
collection that carry `_from` and `_to` (both are `_id` values of the form
`<collection>/<key>`).

## 1. Create the collections

- `createCollection` — `POST /_db/{database-name}/_api/collection`
  - `{"name": "people", "type": 2}` — a document (vertex) collection
  - `{"name": "knows", "type": 3}` — an **edge** collection

`type: 3` is the only thing that makes a collection able to hold edges. Getting
this wrong is the most common first mistake.

## 2. Declare the graph

- `createGraph` — `POST /_db/{database-name}/_api/gharial`

```json
{
  "name": "social",
  "edgeDefinitions": [
    { "collection": "knows", "from": ["people"], "to": ["people"] }
  ],
  "orphanCollections": []
}
```

`from` and `to` are arrays — an edge definition may span several vertex
collections. `orphanCollections` holds vertex collections that are part of the
graph but not yet connected by any edge definition.

Inspect what you built:

- `listGraphs` — `GET /_db/{database-name}/_api/gharial`
- `getGraph` — `GET /_db/{database-name}/_api/gharial/{graph}`
- `listVertexCollections` / `listEdgeCollections`
- `addVertexCollection` — `POST /_db/{database-name}/_api/gharial/{graph}/vertex`
  to add a vertex collection to an existing graph.

## 3. Load vertices and edges

- `createDocuments` — `POST /_db/{database-name}/_api/document/{collection}#multiple`

Insert vertices first, with explicit `_key`s (see
`skills/arangodb-write-documents-safely.md`). Then insert edges whose `_from`
and `_to` are the resulting `_id`s:

```json
[{ "_from": "people/alice", "_to": "people/bob", "since": 2019 }]
```

An edge pointing at a non-existent vertex is accepted at insert time — the graph
does not enforce referential integrity on plain edge collections, so validate on
the client side if you need it.

## 4. Traverse

For a one-hop neighbourhood there is a direct REST operation:

- `getVertexEdges` — `GET /_db/{database-name}/_api/edges/{collection}` with the
  `vertex` and `direction` (`in`/`out`/`any`) query parameters.

For anything deeper, use AQL through the cursor API
(`createAqlQueryCursor`, `POST /_db/{database-name}/_api/cursor`):

```aql
FOR v, e, p IN 1..3 OUTBOUND @start GRAPH @graph
  OPTIONS { uniqueVertices: "path", bfs: true }
  RETURN DISTINCT { vertex: v, depth: LENGTH(p.edges) }
```

with `bindVars: {"start": "people/alice", "graph": "social"}`. Page the result
set exactly as described in `skills/arangodb-run-aql-query.md`.

Bound every traversal: an unbounded `1..n` on a dense graph is the fastest way
to exhaust a server. Set `memoryLimit` on the cursor request as a backstop.

## 5. Tear down

- `deleteGraph` — `DELETE /_db/{database-name}/_api/gharial/{graph}` with
  `dropCollections=true` to remove the underlying collections as well.

## Rules

- `_from`/`_to` must be full `_id` values (`collection/key`), never bare keys.
- Add a persistent index on any vertex attribute you filter on before the
  traversal starts — see `skills/arangodb-search-and-vector-index.md`.
- Branch on `errorNum`; graph errors occupy the 1920–1943 range in
  `errors/arangodb-error-codes.yml`.
