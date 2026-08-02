# ArangoDB

ArangoDB (now operating as **Arango**) is the company behind the open-source,
graph-native multi-model database of the same name, which unifies graph, document,
key/value, vector and full-text search in a single core with one declarative query
language, AQL.

- Website — https://arango.ai/
- Documentation — https://docs.arango.ai/
- HTTP API reference — https://docs.arango.ai/arangodb/stable/develop/http-api/
- GitHub — https://github.com/arangodb
- Status — https://status.arangodb.cloud/

## APIs

| API | Style | Contract |
|---|---|---|
| ArangoDB Core API | REST / HTTP | OpenAPI 3.1.0 — 174 paths, 254 operations, 22 tags (`openapi/`) |
| Arango Managed Platform (AMP) API | gRPC | 28 first-party proto3 service definitions (`grpc/`) |
| AMP SCIM API | REST / SCIM 2.0 | documented |

## Artifacts

| Directory | Contents |
|---|---|
| `openapi/` | ArangoDB Core API 3.12.10, harvested verbatim from `arangodb/arangodb` |
| `grpc/` | 28 ArangoGraph / AMP `.proto` files from `arangodb-managed/apis` |
| `mcp/` | The official ArangoDB MCP server manifest + tool → OpenAPI crosswalk |
| `skills/` | Five packaged Agent Skills grounded in real `operationId`s |
| `errors/` | The full 348-entry ArangoDB `errorNum` registry + HTTP problem view |
| `conventions/` | Cross-cutting request/response semantics (pagination, async, concurrency) |
| `authentication/` | HTTP Basic + JWT (Core), API-key token exchange (AMP), SCIM Basic |
| `packages/` | 14 official client libraries and CLIs across 6 languages |
| `cli/` | ArangoDB client tools, `oasisctl`, `foxx-cli` |
| `lifecycle/`, `changelog/` | Versioning, deprecation and the recent 3.12.x release window |
| `conformance/`, `security/`, `data-model/`, `overlays/`, `well-known/`, `llms/`, `agentic-access/` | supporting artifacts |

Notes recorded honestly rather than filled in: no A2A agent card is published on
any Arango host, no `security.txt`, no trust center or named certifications, no
OAuth surface, no AsyncAPI or webhook surface, and no `Idempotency-Key` contract.
