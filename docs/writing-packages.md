# Writing AX Packages

AX package v0 is intentionally small and source-only. A package is just an `AX.toml` plus AX source files.

## Directory Shape

Use this layout:

```text
packages/
  your_package/
    AX.toml
    README.md
    src/
      module_name.ax
```

Minimal package manifest:

```toml
manifest_version = 1

[package]
name = "your_package"
sources = ["src"]
```

Package source should declare modules under the package name:

```ax
module your_package.rules;

fn score(value: i32) -> i32 {
    return value + 1;
}
```

A user imports it through the dependency alias:

```ax
import your_package.rules;

fn main() -> i32 {
    return your_package.rules.score(1);
}
```

## Local Development

During package development, consume packages as local path dependencies:

```toml
[dependencies]
your_package = { path = "../../packages/your_package" }
```

Then validate with:

```powershell
axc check path\to\example_project
axc run path\to\example_project
```

## Registry Metadata

The AX compiler repository owns the registry metadata. A package entry points back here:

```json
{
  "schema_version": 1,
  "name": "your_package",
  "owner": "ax-core",
  "license": "Apache-2.0",
  "description": "Short package description",
  "versions": [
    {
      "version": "0.1.0",
      "source": {
        "kind": "git",
        "url": "https://github.com/AX-FDN/AX-PKG.git",
        "rev": "<commit>",
        "path": "packages/your_package"
      },
      "checksum": "sha256:<hash>",
      "modules": [
        "your_package.rules"
      ]
    }
  ]
}
```

Generate the checksum with:

```powershell
axc pkg hash packages\your_package
```

## Rules For Preview Packages

- Keep packages source-only.
- Do not add install scripts.
- Do not depend on private files outside the package directory.
- Keep module roots aligned with the dependency alias.
- Prefer pure AX code first. Host/runtime packages may wrap explicit `std.*`
  host APIs, but they must document interpreter/AOT boundaries.
- Add an example project when adding a normal pure AX package.
- Host-boundary packages such as `http_tools` or `net_tools` may use a dedicated
  host smoke instead of the generic `examples/basic_usage` project, because
  they require the AX standard library source root and local services.

## Recommended First Package Types

For AX 0.2 Package Preview, prefer packages that can be validated by `axc check` and `axc run` without host-specific setup:

- Text helpers.
- Validation helpers.
- Number and scoring helpers.
- Report builders.
- Markdown/document inspectors.
- Small collection helpers.
- Pure database connection-string and readiness helpers.
- JSON encoders and deterministic payload builders.
- URL/query helpers.
- Log formatting helpers.
- Authentication header helpers that do not implement cryptography.

Host-boundary preview packages are now acceptable when they wrap explicit
standard-library host APIs:

- HTTP helpers over `std.http`.
- One-shot TCP helpers over `std.net`.
- Database protocol helpers that stay pure AX until TCP/TLS/byte buffers are
  ready.

Do not implement crypto, JWT signing, compression, binary protocols, TLS, or
native database drivers as string-only packages. Those need byte buffers,
runtime/native ABI work, or dedicated audited primitives first.

HTTP and raw TCP preview packages can now wrap `std.http` and `std.net`, but
they are interpreter-first packages until native runtime ABI support exists.
Avoid claiming mature native support for:

- Database drivers.
- Native extensions.
- Packages that shell out to external tools.
- Packages that depend on private local files.

## Adding A Package To This Repository

1. Add `packages/<name>/AX.toml`.
2. Put source files under `packages/<name>/src`.
3. Add a short `packages/<name>/README.md`.
4. Add the package to `examples/basic_usage/AX.toml`.
5. Import at least one module from the package in `examples/basic_usage/src/main.ax`.
6. Run:

```powershell
axc check examples\basic_usage
axc run examples\basic_usage
axc pkg hash packages\<name>
```

After the package source is committed, the AX compiler repository registry can point to that commit, package path, module list, and checksum.
