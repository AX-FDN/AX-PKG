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
- Prefer pure AX code first; host/runtime-heavy packages such as HTTP should wait until the runtime ABI is ready.
- Add an example project when adding a new package.

