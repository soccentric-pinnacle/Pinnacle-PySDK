# Pinnacle-PySDK — specs Remote: `git@github.com:SiliconCentric/Pinnacle-PySDK.git`
Stack: Python, uv-managed, src layout. Consumes the `HalService` gRPC endpoint exposed by Pinnacle-Client — the same wire contract
Pinnacle-Server speaks. ## Layout * `pinnacle-sdk/` — everything: source, tooling, docs, examples, the make workflow (setup/ gen/ check/ build).
* `pinnacle-sdk/src/pinnacle_sdk/` — the package. Import name `pinnacle_sdk`, distribution name `pinnacle-sdk`.
* `pinnacle-sdk/src/pinnacle_sdk/_proto/` and `types/` — generated stubs, ~270 files, **committed on purpose**.
* `acadia_sdk` — deprecation shim re-exporting the public surface for callers on the old name. Lazy PEP 562 `__getattr__`, emits a `DeprecationWarning`, ships in the wheel. ## The regeneration exception This is the one leaf that regenerates protos. Pinnacle-Client and Pinnacle-Server must not, because a
second copy of the generated C++ symbols causes ODR/duplicate-symbol link errors. That is a C++
constraint with no Python equivalent, so it does not apply here.
