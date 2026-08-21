# Pinnacle-PySDK Build and Deployment

## Purpose and scope

Pinnacle-PySDK is a `uv`-managed Python SDK for the Pinnacle HAL gRPC service. The root Makefile creates a virtual environment, installs `pinnacle-sdk[dev]` from `pinnacle-sdk/` in editable mode, regenerates protobuf/dataclass bindings, runs pytest/Ruff/mypy, and builds wheel and source distributions.

## 1. Prerequisites

- Python compatible with `pinnacle-sdk/pyproject.toml` (`>=3.10`).
- `uv` on PATH, or invoke Make with `UV=/path/to/uv`; network access is needed to create/install the environment unless caches are available.
- The proto generation workflow expects the referenced Pinnacle-HAL proto sources and the SDK's generation tools.
- A reachable HAL service and target configuration are required only for live SDK use, not for local packaging checks.

## 2. Setup

Create `.venv` and install the editable package plus dev dependencies:

```bash
make setup
```

Create only the environment if needed:

```bash
make venv
```

The install stamp is `.venv/.stamps/installed`; delete it only when deliberately forcing reinstall.

## 3. Build and compile

Regenerate committed bindings from HAL protobufs:

```bash
make gen
```

Check that generated output is current:

```bash
make gen-check
```

`gen-check` regenerates into the working tree, compares the generated directory, and fails when committed stubs are stale. Wipe and regenerate all generated stubs with:

```bash
make gen-clean
```

These targets can modify generated files and depend on the proto source layout; review diffs before committing.

## 4. Test and lint

```bash
make test                 # pytest with coverage
make lint                 # Ruff
make typecheck            # mypy --strict
make check                # lint + typecheck + test
make                    # default all/check target
```

The Makefile runs tests from `pinnacle-sdk/tests`, lints `src/pinnacle_sdk`, and type-checks with the package's strict mypy configuration.

## 5. Packaging and artifacts

Build the source distribution and wheel:

```bash
make build
```

The target installs the `build` package into the venv and writes artifacts to `pinnacle-sdk/dist/`. No container, OS package, or cloud publication target exists. Inspect the wheel contents and version from `pinnacle-sdk/pyproject.toml` before publishing.

## 6. Deployment, install, and run

For local SDK development, open an activated shell:

```bash
make shell
```

For a consumer environment, install the generated wheel with the consumer's Python package policy, or use the editable setup for development. The root Makefile has no target that installs the SDK onto a device or starts a HAL service. A live example requires the HAL service endpoint and matching `client.conf`/port configuration; verify the endpoint and credentials manually before running any client code.

## 7. Cleanup

```bash
make clean                 # removes package/build/test caches, keeps .venv
make distclean             # clean plus removes .venv and stamps
```

Generated protobuf/type sources are not removed by `clean`; use `make gen-clean` only when a full regeneration is intended.

## 8. Hardware, cloud, and manual verification caveats

- Unit tests and type checks do not prove gRPC interoperability with a running Pinnacle-HAL or physical hardware.
- Live calls require a reachable HAL service, compatible generated contracts, target permissions, and correct service configuration.
- The package manifest includes gRPC/protobuf runtime dependencies; ensure the deployment Python version satisfies `>=3.10`.
- Cloud publishing, device credentials, TLS, and fleet installation are external deployment concerns and are not implemented by this Makefile.
