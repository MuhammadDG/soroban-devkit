# Environment Variables

This page documents every environment variable the `sdkt` CLI reads.
Variables are grouped into **runtime** variables that affect normal CLI use
and **test-harness only** variables that are internal to the crate's own
test suite and are not part of the supported user surface.

## Runtime variables

These variables redirect the local stores that `sdkt` uses. They are useful in
CI jobs, containers, shared runners, and environments with a read-only home
directory.

### `SDDKT_CACHE_DIR`

Overrides the root of the WASM cache store.

- **Default when unset:** the OS cache directory:
  - Linux: `~/.cache/soroban-devkit/`
  - macOS: `~/Library/Caches/org.SaboLabs.soroban-devkit/`
  - Windows: `%LOCALAPPDATA%\SaboLabs\soroban-devkit\cache\`

Example:

```bash
SDKT_CACHE_DIR=/tmp/cdtest sdkt wasm cache info
```

### `SDDKT_IDENTITY_DIR`

Overrides the directory where the CLI stores identity material.

- **Default when unset:** the OS data directory for `soroban-devkit`.

Example:

```bash
SDKT_IDENTITY_DIR=/tmp/idtest sdkt identity list
```

### `SDDKT_NETWORK_DIR`

Overrides the directory where saved network profiles are stored.

- **Default when unset:** the OS data directory for `soroban-devkit`.

Example:

```bash
SDKT_NETWORK_DIR=/tmp/ndtest sdkt network add localnet \
  -r https://example.org \
  -p 'Test DF Network ; September 2015'
```

### `SDDKT_PLUGIN_DIR`

Overrides the directory where audit plugins are stored.

- **Default when unset:** the OS data directory for `soroban-devkit`.

Example:

```bash
SDKT_PLUGIN_DIR=/tmp/plugins sdkt audit plugin list
```

## Test-harness only variables

> **Not for end users.** These variables are read by the `sdkt-fuzz` test
> suite to coordinate live differential and cross-process determinism tests.
> They are not part of the supported CLI surface and may change without notice.

The following variables are read only from the crate's integration tests:

- `SDDKT_DIFFERENTIAL_FUNCTION` -- selects the function used by the live
  differential test.
- `SDKT_DIFFERENTIAL_SOURCE` -- selects the source contract used by the
  live differential test.
- `SDDKT_SIM_CAPTURE` -- enables capture of simulation output in the live
  differential test.
- `SDKT_SIM_DEBUG` -- enables debug output from the simulation harness.
- `SDKT_FUZZ_CHILD_DIGEST` -- passes the expected digest to a cross-process
  determinism child process.
- `SDKT_FUZZ_PI_CHILD` -- marks a protocol-initial-budget child process.
