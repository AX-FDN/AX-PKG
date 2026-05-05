# http_tools

Small HTTP helpers for AX projects.

This package wraps `std.http`, so consuming projects must include the AX standard
library source root in `AX.toml` and should currently treat HTTP as an
interpreter-first host runtime capability.

Current boundary:

- Pure request/status/header helpers are thin wrappers over `std.http` and do
  not perform network I/O.
- Plain `http://` GET only through `std.http`.
- No HTTPS/TLS yet.
- No POST, custom headers, redirects, streaming, or binary bodies yet.
- `axc build` should report a `host_http` / `AOT0301` readiness blocker until
  the native runtime ABI supports HTTP.
