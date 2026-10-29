# ArangoDB

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

ArangoDB (now operating as Arango) is the company behind the open-source, graph-native multi-model database of the same name, which unifies graph, document, key/value, vector and full-text search in a single core with one declarative query language, AQL. The database exposes its entire feature surface over a RESTful HTTP API documented with OpenAPI 3.1, and the company also runs the Arango Managed Platform (AMP, formerly ArangoGraph Insights Platform / Oasis) — a managed cloud on AWS, GCP and Azure whose control plane is a gRPC/Protobuf API at api.cloud.arangodb.com with a companion oasisctl CLI. Arango publishes official drivers for JavaScript, Python, Java, Go and PHP, an official Model Context Protocol server for AQL generation, and a Kubernetes operator.

**Areas:** Company · Database · Graph Database · Multi-Model Database · NoSQL · Vector Search · Knowledge Graph · Developer Tools · Cloud Infrastructure · Artificial Intelligence

## ArangoDB on APIs.io

Everything profiled for ArangoDB is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/arangodb/](https://apis.io/providers/arangodb/)** — or go straight to one of the APIs or properties below.

### APIs (24)

- [Arango Managed Platform (AMP) API](https://apis.io/apis/arangodb/amp-api/) — The control-plane API of the Arango Managed Platform (formerly ArangoGraph Insights Platform / Oasis) — the managed ArangoDB cloud. It is a gRPC API defined…
- [Arango Managed Platform SCIM API](https://apis.io/apis/arangodb/amp-scim-api/) — SCIM 2.0 user and group provisioning endpoint for Arango Managed Platform organizations, used to synchronize identities from an external identity provider…
- [ArangoDB Administration API](https://apis.io/apis/arangodb/arangodb-administration-api/) — Get server information, manage licenses, shut down nodes, and more
- [ArangoDB Analyzers API](https://apis.io/apis/arangodb/arangodb-analyzers-api/) — Manage Analyzers for transforming data
- [ArangoDB Authentication API](https://apis.io/apis/arangodb/arangodb-authentication-api/) — Manage session tokens and JWT secrets
- [ArangoDB Batch Requests API](https://apis.io/apis/arangodb/arangodb-batch-requests-api/) — Run multiple operations using a single request
- [ArangoDB Cluster API](https://apis.io/apis/arangodb/arangodb-cluster-api/) — Get information, monitor, and administrate cluster deployments
- [ArangoDB Collections API](https://apis.io/apis/arangodb/arangodb-collections-api/) — Manage collections for organizing documents
- [ArangoDB Databases API](https://apis.io/apis/arangodb/arangodb-databases-api/) — Manage databases for organizing collections
- [ArangoDB Documents API](https://apis.io/apis/arangodb/arangodb-documents-api/) — Perform CRUD operations on JSON-based records
- [ArangoDB Foxx API](https://apis.io/apis/arangodb/arangodb-foxx-api/) — Manage microservices written in JavaScript
- [ArangoDB Graphs API](https://apis.io/apis/arangodb/arangodb-graphs-api/) — Manage named graphs and query edges
- [ArangoDB Hot Backups API](https://apis.io/apis/arangodb/arangodb-hot-backups-api/) — Manage incremental data backups
- [ArangoDB Import API](https://apis.io/apis/arangodb/arangodb-import-api/) — Load JSON data in bulk
- [ArangoDB Indexes API](https://apis.io/apis/arangodb/arangodb-indexes-api/) — Improve the performance of queries
- [ArangoDB Jobs API](https://apis.io/apis/arangodb/arangodb-jobs-api/) — Execute requests asynchronously
- [ArangoDB Monitoring API](https://apis.io/apis/arangodb/arangodb-monitoring-api/) — Access logs, statistics, and metrics
- [ArangoDB Queries API](https://apis.io/apis/arangodb/arangodb-queries-api/) — Run, process, and manage AQL queries
- [ArangoDB Replication API](https://apis.io/apis/arangodb/arangodb-replication-api/) — Control data replication for deployments
- [ArangoDB Security API](https://apis.io/apis/arangodb/arangodb-security-api/) — Configure audit logging, encryption at rest and encryption in transit
- [ArangoDB Tasks API](https://apis.io/apis/arangodb/arangodb-tasks-api/) — Set up JavaScript code to run periodically or timed
- [ArangoDB Transactions API](https://apis.io/apis/arangodb/arangodb-transactions-api/) — Execute JavaScript and Stream Transactions
- [ArangoDB Users API](https://apis.io/apis/arangodb/arangodb-users-api/) — Manage ArangoDB user accounts
- [ArangoDB Views API](https://apis.io/apis/arangodb/arangodb-views-api/) — Manage Views to use ArangoSearch for information retrieval

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [ArangoDB Model Context Protocol (MCP) Server](https://apis.io/servers/arangodb/arangodb/)

#### Agentic access (1)

Which operations an agent can call, which act, and which need a human in the loop.

- [Arangodb Agentic Access](https://apis.io/agentic-access/arangodb/arangodb-agentic-access/)

#### Collections (22)

Postman and Open Collection request collections.

- [ArangoDB Core Administration API](https://apis.io/collections/arangodb/open-arangodb-administration-api/)
- [ArangoDB Core Analyzers API](https://apis.io/collections/arangodb/open-arangodb-analyzers-api/)
- [ArangoDB Core Authentication API](https://apis.io/collections/arangodb/open-arangodb-authentication-api/)
- [ArangoDB Core Batch Requests API](https://apis.io/collections/arangodb/open-arangodb-batch-requests-api/)
- [ArangoDB Core Cluster API](https://apis.io/collections/arangodb/open-arangodb-cluster-api/)
- [ArangoDB Core Collections API](https://apis.io/collections/arangodb/open-arangodb-collections-api/)
- [ArangoDB Core Databases API](https://apis.io/collections/arangodb/open-arangodb-databases-api/)
- [ArangoDB Core Documents API](https://apis.io/collections/arangodb/open-arangodb-documents-api/)
- [ArangoDB Core Foxx API](https://apis.io/collections/arangodb/open-arangodb-foxx-api/)
- [ArangoDB Core Graphs API](https://apis.io/collections/arangodb/open-arangodb-graphs-api/)
- [ArangoDB Core Hot Backups API](https://apis.io/collections/arangodb/open-arangodb-hot-backups-api/)
- [ArangoDB Core Import API](https://apis.io/collections/arangodb/open-arangodb-import-api/)
- [ArangoDB Core Indexes API](https://apis.io/collections/arangodb/open-arangodb-indexes-api/)
- [ArangoDB Core Jobs API](https://apis.io/collections/arangodb/open-arangodb-jobs-api/)
- [ArangoDB Core Monitoring API](https://apis.io/collections/arangodb/open-arangodb-monitoring-api/)
- [ArangoDB Core Queries API](https://apis.io/collections/arangodb/open-arangodb-queries-api/)
- [ArangoDB Core Replication API](https://apis.io/collections/arangodb/open-arangodb-replication-api/)
- [ArangoDB Core Security API](https://apis.io/collections/arangodb/open-arangodb-security-api/)
- [ArangoDB Core Tasks API](https://apis.io/collections/arangodb/open-arangodb-tasks-api/)
- [ArangoDB Core Transactions API](https://apis.io/collections/arangodb/open-arangodb-transactions-api/)
- [ArangoDB Core Users API](https://apis.io/collections/arangodb/open-arangodb-users-api/)
- [ArangoDB Core Views API](https://apis.io/collections/arangodb/open-arangodb-views-api/)

#### Overlays (1)

OpenAPI overlays, including conversational phrasing for each operation.

- [Arangodb Core Overlay](https://apis.io/overlays/arangodb/arangodb-core-overlay/)

#### Security (2)

Authentication, trust centre, and vulnerability disclosure.

- [Arangodb Authentication](https://apis.io/security/arangodb/arangodb-authentication/)
- [Arangodb Domain Security](https://apis.io/security/arangodb/arangodb-domain-security/)

#### Plans and pricing (1)

Published plans, tiers and prices.

- [Arangodb Plans Pricing](https://apis.io/plans/arangodb/arangodb-plans-pricing/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://arango.ai/)
- [Documentation](https://docs.arango.ai/)
- [Getting started](https://docs.arango.ai/arangodb/stable/get-started/)
- [API reference](https://docs.arango.ai/arangodb/stable/develop/http-api/)
- [Pricing](https://arango.ai/pricing/)
- [Sign up](https://dashboard.arangodb.cloud/)
- [Support](https://arango.ai/contact-us/)
- [Blog](https://arango.ai/blog/)
- [Privacy policy](https://arango.ai/privacy-policy/)
- [GitHub](https://github.com/arangodb)
- [Status](https://status.arangodb.cloud/)

## API Evangelist rating

**Kin Score 52.7** — developing · rubric 0.25.0 · scored 2026-10-23

| Facet | Score |
|---|---|
| Discoverability | 73.2 |
| Contract quality | 42.3 |
| Contract governance | 4.5 |
| Operational transparency | 42.1 |
| Developer ergonomics | 73.2 |
| Access clarity | 65.8 |

**Agent readiness 28.3** — agent-aware

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | derived (partial credit) |
| Reversibility Documented | documented (partial credit) |
| MCP Server | — |
| Auth Clarity | bearer (partial credit) |
| Idempotency | — |
| Error Semantics | documented (partial credit) |
| OpenAPI Examples | verified |
| Rate Limit Signal | — |
| Event Surface Described | — |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | — |
| Delegated Identity | — |
| Protected Resource Metadata | — |
| Dynamic Client Registration | — |
| Agentic Commerce | — |
| Pagination Ratio | — |
| Operation Distinctiveness | distinct |

**Regulatory alignment 19.1** — Horizontal (data, software, accessibility, platform)

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/arangodb/](https://apis.io/providers/arangodb/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for ArangoDB: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/arangodb/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
