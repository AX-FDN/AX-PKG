# report_tools

String report builders for small command-line tools.

## Modules

- `report_tools.builder`

## Example

```ax
import report_tools.builder;

fn main() -> i32 {
    println(report_tools.builder.kv_i32("", "score", 9));
    return 0;
}
```
