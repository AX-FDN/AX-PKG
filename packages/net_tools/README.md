# net_tools

Small TCP helpers for AX package experiments.

This package wraps `std.net`, so consuming projects must include the AX standard
library source root in `AX.toml`. It is intended for interpreter-first protocol
experiments and local smoke tests.

Current boundary:

- One-shot TCP exchange only.
- No TLS yet.
- No socket handles, async IO, streaming, or binary buffers yet.
- `axc build` should report a `host_net` / `AOT0301` readiness blocker until the
  native runtime ABI supports sockets.
