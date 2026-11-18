# auth-service-backend

Identity and tokens.

Part of [Finance Master](https://github.com/financemaster-proj/platform) — a
personal finance system built as four backend services behind an API gateway.

## What this service owns

Users, credentials, roles and refresh tokens. This is the **only** service that can mint a token — it holds the RSA private key and publishes the public key at `/.well-known/jwks.json` for every other service to validate against locally.

## What it does not own

Any financial data. No expense, holding or report ever lives here.

## Interface

| | |
|---|---|
| Port | `8081` |
| Database | `fm_auth` |
| Emits | `UserRegistered`, `UserDeleted` |
| Consumes | — |

## Running locally

Infrastructure lives in the [`platform`](https://github.com/financemaster-proj/platform)
repo. Clone it as a sibling of this one, then:

```bash
cd ../platform && docker compose -f infra/docker-compose.yml up -d
cd - && cp .env.example .env && ./gradlew bootRun
```

## Conventions

This service follows the project-wide rules in
[`platform/docs`](https://github.com/financemaster-proj/platform/tree/main/docs) —
in particular **no synchronous calls to another service**
([ADR-0004](https://github.com/financemaster-proj/platform/blob/main/docs/adr/0004-no-synchronous-service-calls.md)),
package by feature, Flyway for every schema change, and money as `BigDecimal`
with an explicit currency.
