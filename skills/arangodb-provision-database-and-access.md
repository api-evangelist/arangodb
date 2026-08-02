---
name: Provision an ArangoDB database and grant least-privilege access
description: >-
  Create a database, create a service user, and grant it exactly the database
  and collection permissions it needs — instead of handing an integration the
  root credentials.
api: openapi/arangodb-core-openapi-original.json
generated: '2026-08-02'
method: generated
operations:
  - listDatabases
  - createDatabase
  - getCurrentDatabase
  - createUser
  - getUser
  - listUsers
  - setUserDatabasePermissions
  - getUserDatabasePermissions
  - setUserCollectionPermissions
  - getUserCollectionPermissions
  - deleteUserDatabasePermissions
  - listUserAccessibleDatabases
  - deleteDatabase
---

# Provision a database and grant least-privilege access

ArangoDB authorization is not scopes or roles — it is a two-level permission
grid (database, then collection) with three grades: `rw`, `ro`, `none`.

## 1. Create the database

All database management is done against `_system`:

- `listDatabases` — `GET /_db/_system/_api/database`
- `createDatabase` — `POST /_db/_system/_api/database` with `{"name": "app"}`
- `getCurrentDatabase` — `GET /_db/{database-name}/_api/database/current` to
  confirm which database a connection is actually bound to.

Creating a database that exists returns `409` / `errorNum 1207`.

## 2. Create a service user

- `createUser` — `POST /_db/{database-name}/_api/user` with
  `{"user": "app-svc", "passwd": "…", "active": true, "extra": {...}}`
- `listUsers` / `getUser` to verify.

Do not reuse `root`. `root` has access to every database on the deployment and
there is no way to scope it down.

## 3. Grant database-level access

- `setUserDatabasePermissions` —
  `PUT /_db/{database-name}/_api/user/{user}/database/{dbname}` with
  `{"grant": "rw" | "ro" | "none"}`
- `getUserDatabasePermissions` to read it back.
- `deleteUserDatabasePermissions` (`DELETE`) removes the explicit grant and
  falls back to the wildcard default.

Start at `ro` and raise only where the integration genuinely writes.

## 4. Narrow to specific collections

- `setUserCollectionPermissions` —
  `PUT /_db/{database-name}/_api/user/{user}/database/{dbname}/{collection}`
- `getUserCollectionPermissions` to verify.

Collection grants override the database grant for that collection. The common
shape for a read-mostly integration: database `ro`, plus `rw` on the one or two
collections it writes.

## 5. Verify from the consumer's side

- `listUserAccessibleDatabases` —
  `GET /_db/{database-name}/_api/user/{user}/database?full=true` returns the
  effective permission map. Check this rather than assuming the grants applied.

Then authenticate as the new user (`createSessionToken`,
`POST /_open/auth`) and confirm a forbidden call really returns `403` /
`errorNum 11` (`ERROR_FORBIDDEN`).

## 6. Tear down

- `deleteDatabase` — `DELETE /_db/_system/_api/database/{database-name}`.
  This is irreversible and drops every collection in it. Take a hot backup first
  (`createBackup`, `POST /_admin/backup/create`).

## Rules

- Server-side enforcement depends on `--server.authentication true` (the
  default). With `--server.authentication-system-only true` (also the default),
  Foxx services do **not** require authentication — deploy nothing sensitive as
  a Foxx service without reading
  `authentication/arangodb-authentication.yml` first.
- On the Arango Managed Platform this Core-API user grid sits *underneath* the
  AMP organization/project/deployment role bindings; they are two separate
  authorization systems.
- Every failure here surfaces as `403` / `errorNum 11` or `401` / `errorNum 11`
  — check the HTTP status alongside `errorNum`.
