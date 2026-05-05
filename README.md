# AX-PKG

Curated source package repository for AX.

This repository is a package monorepo. Each package lives under `packages/<name>` and is referenced by AX registry metadata through `source.path`.

```text
packages/
  text_tools/
  config_rules/
  math_rules/
  result_tools/
```

The AX compiler repository stores registry metadata, while this repository stores package source.

## Current Preview Packages

| Package | Modules | Purpose |
| --- | --- | --- |
| `text_tools` | `text_tools.normalize`, `text_tools.stats` | Text normalization and simple text metrics |
| `config_rules` | `config_rules.validate` | Key-value config validation helpers |
| `math_rules` | `math_rules.core` | Small integer scoring helpers |
| `result_tools` | `result_tools.summary` | Helpers around `std.result.Result<i32, string>` |

## Validate Locally

From the AX compiler repository, run:

```powershell
D:\CargoTarget\AX\debug\axc.exe check ..\AX-PKG\examples\basic_usage
D:\CargoTarget\AX\debug\axc.exe run ..\AX-PKG\examples\basic_usage
```

To compute a registry checksum for a package:

```powershell
D:\CargoTarget\AX\debug\axc.exe pkg hash ..\AX-PKG\packages\text_tools
```

## Writing Packages

See [docs/writing-packages.md](docs/writing-packages.md).

