# Pinnacle-PySDK

## Project Summary

Pinnacle-PySDK is a typed Python client for the Pinnacle HAL gRPC contract.

The package under `pinnacle-sdk` uses `pyproject.toml` with pytest and coverage tooling; generated protobuf modules live under private `_proto`.

Hand-written code exposes a curated API rather than raw protobuf messages. Callers can:

- Connect to a device and discover peripherals.
- Run single-shot tests.
- Start monitor, event, or long-running sessions.
- Pause, resume, or stop sessions.
- Convert results into Python dataclasses.

Type generation is driven by `tools/gen_types.py`, and generated module types cover the HAL registry peripheral set.

Tests cover connections, discovery, one-shot results and errors, monitor and event iteration, long-running lifecycle, and type round trips.

The repository audit reports 35 collected tests passing against a real in-process FakeServer and notes that generated dataclasses depress aggregate coverage despite meaningful hand-written runtime coverage.

This validates gRPC calls and conversions without proving compatibility with the C++ Pinnacle-Client, real HAL probes, board metrics, authentication, or network faults.

The documented contract has boundaries:

- The default endpoint is plaintext port 50051.
- There is no global wire contract-version field.
- Capabilities are currently uniform across modules.
- Metric semantics are runtime key names rather than machine-readable unit metadata.

These limitations are recorded in `docs/SDK_FINDINGS.md`. Use the package Makefile for installation, tests, type generation checks, linting, and strict typing. Run a Client/board end-to-end test before publishing a release.

## Feature Status

| # | Name / Description | Implemented? | Test Coverage? | Manual Test |
|---:|---|---|---|---|
| 1 | Typed gRPC connection and curated public API | Yes | Connection tests and generated-stub checks are present | [ ] Manual verification required |
| 2 | Peripheral discovery and capability surface | Yes | Discovery tests cover active, unknown, and capability cases | [ ] Manual verification required |
| 3 | Single-shot and self-test calls | Yes | One-shot tests cover results, errors, timeout, config, and named tests | [ ] Manual verification required |
| 4 | Monitor, event, and long-running sessions | Yes | Dedicated monitor, event, and lifecycle tests | [ ] Manual verification required |
| 5 | Pause, resume, stop, and session controls | Yes | Long-running tests cover lifecycle and controls | [ ] Manual verification required |
| 6 | Protobuf-to-dataclass conversion and type generation | Yes | Type round-trip tests and `gen-check` tooling | [ ] Manual verification required |
| 7 | Real C++ Client and board interoperability | Partial | FakeServer contract tests only; deployed E2E evidence is absent | [ ] Manual verification required |

## Tests and Commands

```bash
make test
make lint
```

## Limitations

- The SDK does not validate hardware metrics, production transport, or a real Client.
- Contract-version negotiation and machine-readable metric units remain documented gaps.
