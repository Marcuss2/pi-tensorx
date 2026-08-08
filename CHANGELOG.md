# Changelog

## Unreleased

- Register via pi's `refreshModels` callback instead of a factory-time env-only fetch, so the live `/v1/model/info` catalog is fetched **with the authenticated key** — including the `/login`-stored key that pi passes as the effective credential. Previously the provider only fetched live when `TENSORX_API_KEY` was set, so `/login`-authenticated users were stuck with the bundled snapshot, which had gone stale: TensorX retired `deepseek-v4-flash` (renamed to `deepseek-v4-flash-0731`) and dropped several models, so pi requested a retired id and the router returned 403 `permission_error`. The snapshot now only serves as the initial/offline catalog and is regenerated from `GET /v1/model/info`.
- Raise the minimum pi version: `refreshModels` requires `@earendil-works/pi-coding-agent >= 0.81.0` (was `*`). Dependencies updated accordingly.

## 1.0.0 - 2026-06-28

Initial release.

- Registers TensorX as a pi provider.
- Fetches the TensorX model catalog (`GET /v1/model/info`) on startup when `TENSORX_API_KEY` is set, and falls back to a bundled catalog snapshot otherwise so the provider still appears under `/login` → API Keys.
- Registers tool-capable models with context, pricing, image, and reasoning metadata.
- Supports pi's API-key login flow and the `TENSORX_API_KEY` environment variable.
- Adds `/tensorx-models` to list available TensorX models.
