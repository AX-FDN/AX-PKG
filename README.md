# AX-PKG

Curated source package repository for AX.

This repository is a package monorepo. Each package lives under `packages/<name>` and is referenced by AX registry metadata through `source.path`.

```text
packages/
  api_tools/
  auth_tools/
  bytes_tools/
  cache_tools/
  collection_tools/
  database_tools/
  encoding_tools/
  feature_flag_tools/
  hash_tools/
  health_tools/
  json_tools/
  jwt_tools/
  log_tools/
  text_tools/
  config_rules/
  math_rules/
  migration_tools/
  http_tools/
  url_tools/
  net_tools/
  number_tools/
  observability_tools/
  pagination_tools/
  queue_tools/
  rate_limit_tools/
  markdown_tools/
  report_tools/
  result_tools/
  retry_tools/
  schema_tools/
  validation_tools/
```

The AX compiler repository stores registry metadata, while this repository stores package source.

## Current Preview Packages

| Package | Modules | Purpose |
| --- | --- | --- |
| `api_tools` | `api_tools.response` | API response envelope, status, and request metadata helpers |
| `auth_tools` | `auth_tools.headers` | Bearer/API-key header helpers and safe secret redaction |
| `bytes_tools` | `bytes_tools.core` | Byte-buffer helpers over `std.bytes` for backend-oriented packages |
| `cache_tools` | `cache_tools.keys` | Cache key and freshness policy helpers |
| `collection_tools` | `collection_tools.ints` | Integer slice summaries and aggregates |
| `database_tools` | `database_tools.dsn` | Pure AX DSN classification and database readiness helpers |
| `encoding_tools` | `encoding_tools.core` | Hex and base64 helpers over `std.bytes` |
| `feature_flag_tools` | `feature_flag_tools.flags` | Feature flag rollout and decision helpers |
| `hash_tools` | `hash_tools.checksum` | Non-cryptographic deterministic checksum helpers |
| `health_tools` | `health_tools.checks` | Service readiness and dependency health helpers |
| `json_tools` | `json_tools.encode` | Deterministic JSON string construction helpers |
| `jwt_tools` | `jwt_tools.preview` | Unsigned JWT shape helpers for package experiments |
| `log_tools` | `log_tools.core` | Structured log line formatting helpers |
| `text_tools` | `text_tools.normalize`, `text_tools.stats` | Text normalization and simple text metrics |
| `config_rules` | `config_rules.validate` | Key-value config validation helpers |
| `math_rules` | `math_rules.core` | Small integer scoring helpers |
| `http_tools` | `http_tools.client` | Interpreter-backed plain HTTP GET helpers over `std.http` |
| `net_tools` | `net_tools.tcp` | Interpreter-backed one-shot TCP helpers over `std.net` |
| `url_tools` | `url_tools.core` | URL classification and query construction helpers |
| `number_tools` | `number_tools.core` | Integer clamps, percentages, and range checks |
| `observability_tools` | `observability_tools.signals` | Metric, span, and latency signal helpers |
| `pagination_tools` | `pagination_tools.core` | Page, offset, and window helpers |
| `markdown_tools` | `markdown_tools.headings` | Markdown heading inspection helpers |
| `migration_tools` | `migration_tools.plan` | Migration naming, batch status, and rollback helpers |
| `report_tools` | `report_tools.builder` | Plain-text report construction helpers |
| `queue_tools` | `queue_tools.jobs` | Queue job status, retry, and dead-letter helpers |
| `rate_limit_tools` | `rate_limit_tools.window` | Rate-limit quota and window helpers |
| `result_tools` | `result_tools.summary` | Helpers around `std.result.Result<i32, string>` |
| `retry_tools` | `retry_tools.policy` | Retry classification and delay policy helpers |
| `schema_tools` | `schema_tools.describe` | Schema field and table description helpers |
| `validation_tools` | `validation_tools.rules` | Reusable validation predicates and status messages |

## Validate Locally

From the AX compiler repository, run:

```powershell
D:\CargoTarget\AX\debug\axc.exe check ..\AX-PKG\examples\basic_usage
D:\CargoTarget\AX\debug\axc.exe run ..\AX-PKG\examples\basic_usage
D:\CargoTarget\AX\debug\axc.exe check ..\AX-PKG\examples\bytes_usage
D:\CargoTarget\AX\debug\axc.exe run ..\AX-PKG\examples\bytes_usage
D:\CargoTarget\AX\debug\axc.exe check ..\AX-PKG\examples\http_helpers
D:\CargoTarget\AX\debug\axc.exe run ..\AX-PKG\examples\http_helpers
```

To compute a registry checksum for a package:

```powershell
D:\CargoTarget\AX\debug\axc.exe pkg hash ..\AX-PKG\packages\text_tools
```

To validate every package shape quickly, run the example project. It imports the preview packages through local path dependencies so package authors can test changes before registry metadata is updated.

## How To Contribute

AX-PKG uses a curated contribution model. Package source lives in this repository, while registry metadata lives in the AX compiler repository. In normal contribution flow, a package PR starts here first; after it is accepted and committed, the AX registry can point to the new package path, commit rev, checksum, and module list.

Good first contributions are source-only packages that can pass `axc check` and `axc run` without private services, local secrets, native extensions, or special machine setup. The best packages for the current preview are small, boring, useful building blocks:

- backend workflow helpers
- text, JSON, URL, config, report, validation, and collection utilities
- API response, pagination, retry, cache, queue, health, and observability helpers
- database metadata helpers that do not pretend to be real drivers
- authentication and token-shape helpers that do not implement cryptographic signing
- byte/encoding helpers built on `std.bytes`, as long as they are clearly documented

Please avoid PRs that claim production-grade security or native behavior before AX has the required runtime/backend foundation:

- no real crypto, password hashing, JWT signing, TLS, or MAC packages yet
- no native database drivers yet
- no install scripts or build scripts
- no packages that shell out to private local tools
- no packages that require secrets, accounts, cloud credentials, or external paid services
- no binary artifacts or vendored generated output

### Package Shape

Add packages under `packages/<name>`:

```text
packages/
  your_package/
    AX.toml
    README.md
    src/
      module.ax
```

Use a source-only manifest:

```toml
manifest_version = 1

[package]
name = "your_package"
sources = ["src"]
```

Module names should start with the package name:

```ax
module your_package.rules;

fn score(value: i32) -> i32 {
    return value + 1;
}
```

### Example Coverage

For stable pure AX packages, add the package to:

- `examples/basic_usage/AX.toml`
- `examples/basic_usage/src/main.ax`

The example should import at least one module and exercise at least one useful function. Keep the output deterministic so the AX compiler repository can use it in registry smoke tests.

If the package wraps host-boundary APIs such as `std.http` or `std.net`, do not force it into `examples/basic_usage`. Add or update a focused example/smoke instead and document that the package is interpreter-first until native runtime ABI support exists.

### Local Validation

From the AX compiler repository, validate your package through this repository:

```powershell
D:\CargoTarget\AX\debug\axc.exe check ..\AX-PKG\examples\basic_usage
D:\CargoTarget\AX\debug\axc.exe run ..\AX-PKG\examples\basic_usage
D:\CargoTarget\AX\debug\axc.exe pkg hash ..\AX-PKG\packages\your_package
```

If your package has a dedicated example, run that too:

```powershell
D:\CargoTarget\AX\debug\axc.exe check ..\AX-PKG\examples\your_example
D:\CargoTarget\AX\debug\axc.exe run ..\AX-PKG\examples\your_example
```

### PR Checklist

Before opening a PR, make sure it includes:

- package source under `packages/<name>`
- a short package README with purpose, modules, and boundaries
- example coverage for pure AX packages
- no private files, generated binaries, credentials, or install scripts
- deterministic output if the package is included in `examples/basic_usage`
- a package checksum from `axc pkg hash`
- a clear note if the package is interpreter-first or host-boundary

After the package source is merged, registry metadata can be added in the AX compiler repository under `registry/packages/<name>.json` and `registry/index.json`. The registry entry should pin the AX-PKG commit rev and checksum for reproducible installs.

## Writing Packages

See [docs/writing-packages.md](docs/writing-packages.md).
