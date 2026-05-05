# AX-PKG

Curated source package repository for AX.

This repository is a package monorepo. Each package lives under `packages/<name>` and is referenced by AX registry metadata through `source.path`.

```text
packages/
  auth_tools/
  bytes_tools/
  collection_tools/
  database_tools/
  json_tools/
  log_tools/
  text_tools/
  config_rules/
  math_rules/
  http_tools/
  url_tools/
  net_tools/
  number_tools/
  markdown_tools/
  report_tools/
  result_tools/
  validation_tools/
```

The AX compiler repository stores registry metadata, while this repository stores package source.

## Current Preview Packages

| Package | Modules | Purpose |
| --- | --- | --- |
| `auth_tools` | `auth_tools.headers` | Bearer/API-key header helpers and safe secret redaction |
| `bytes_tools` | `bytes_tools.core` | Byte-buffer helpers over `std.bytes` for backend-oriented packages |
| `collection_tools` | `collection_tools.ints` | Integer slice summaries and aggregates |
| `database_tools` | `database_tools.dsn` | Pure AX DSN classification and database readiness helpers |
| `json_tools` | `json_tools.encode` | Deterministic JSON string construction helpers |
| `log_tools` | `log_tools.core` | Structured log line formatting helpers |
| `text_tools` | `text_tools.normalize`, `text_tools.stats` | Text normalization and simple text metrics |
| `config_rules` | `config_rules.validate` | Key-value config validation helpers |
| `math_rules` | `math_rules.core` | Small integer scoring helpers |
| `http_tools` | `http_tools.client` | Interpreter-backed plain HTTP GET helpers over `std.http` |
| `net_tools` | `net_tools.tcp` | Interpreter-backed one-shot TCP helpers over `std.net` |
| `url_tools` | `url_tools.core` | URL classification and query construction helpers |
| `number_tools` | `number_tools.core` | Integer clamps, percentages, and range checks |
| `markdown_tools` | `markdown_tools.headings` | Markdown heading inspection helpers |
| `report_tools` | `report_tools.builder` | Plain-text report construction helpers |
| `result_tools` | `result_tools.summary` | Helpers around `std.result.Result<i32, string>` |
| `validation_tools` | `validation_tools.rules` | Reusable validation predicates and status messages |

## Validate Locally

From the AX compiler repository, run:

```powershell
D:\CargoTarget\AX\debug\axc.exe check ..\AX-PKG\examples\basic_usage
D:\CargoTarget\AX\debug\axc.exe run ..\AX-PKG\examples\basic_usage
D:\CargoTarget\AX\debug\axc.exe check ..\AX-PKG\examples\bytes_usage
D:\CargoTarget\AX\debug\axc.exe run ..\AX-PKG\examples\bytes_usage
```

To compute a registry checksum for a package:

```powershell
D:\CargoTarget\AX\debug\axc.exe pkg hash ..\AX-PKG\packages\text_tools
```

To validate every package shape quickly, run the example project. It imports the preview packages through local path dependencies so package authors can test changes before registry metadata is updated.

## Writing Packages

See [docs/writing-packages.md](docs/writing-packages.md).
