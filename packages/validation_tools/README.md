# validation_tools

Reusable validation predicates and stable status-code helpers.

## Modules

- `validation_tools.rules`

## Example

```ax
import validation_tools.rules;

fn main() -> i32 {
    return validation_tools.rules.require_min_length("abc", 2);
}
```
