# number_tools

Small integer helpers for scoring, clamping, percentages, and range checks.

## Modules

- `number_tools.core`

## Example

```ax
import number_tools.core;

fn main() -> i32 {
    return number_tools.core.clamp_i32(12, 0, 10);
}
```
