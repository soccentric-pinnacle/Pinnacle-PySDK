# Pinnacle-PySDK Python SDK leaf for the **Pinnacle** project (NXP i.MX 93 + STM32) in the
SiliconCentric `project-managers` fleet. This leaf provides `pinnacle-sdk`, a Pythonic gRPC client for the `HalService`
endpoint exposed by `Pinnacle-Client`. The SDK distribution package is
**`pinnacle-sdk`** (import name `pinnacle_sdk`); a `acadia_sdk` deprecation shim
re-exports the public surface for callers still on the old name. The SDK source, tooling, docs, examples, and test suite live under
[`pinnacle-sdk/`](pinnacle-sdk). See [`pinnacle-sdk/README.md`](pinnacle-sdk/README.md)
for the full API, quickstart, and `make` targets (`setup`, `gen`, `check`,
`build`). ---
Part of the `Pinnacle` project in the `project-managers` fleet. See
`docs/project-managers.md` for the full 9-project/ 8-leaf hierarchy and
shared conventions.