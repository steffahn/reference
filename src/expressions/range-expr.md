
r[expr.range]
# Range expressions

r[expr.range.syntax]
```grammar,expressions
RangeExpression ->
      RangeExpr
    | RangeFromExpr
    | RangeToExpr
    | RangeFullExpr
    | RangeInclusiveExpr
    | RangeToInclusiveExpr

RangeExpr -> Expression `..` Expression

RangeFromExpr -> Expression `..`

RangeToExpr -> `..` Expression

RangeFullExpr -> `..`

RangeInclusiveExpr -> Expression `..=` Expression

RangeToInclusiveExpr -> `..=` Expression
```

r[expr.range.behavior]
The `..` and `..=` operators will construct an object of one of the `std::ops::Range` (or `core::ops::Range`) variants, according to the following table:

| Production             | Syntax        | Type                         | Range                 |
|------------------------|---------------|------------------------------|-----------------------|
| _RangeExpr_            | start`..`end  | [std::ops::Range]            | start &le; x &lt; end |
| _RangeFromExpr_        | start`..`     | [std::ops::RangeFrom]        | start &le; x          |
| _RangeToExpr_          | `..`end       | [std::ops::RangeTo]          |            x &lt; end |
| _RangeFullExpr_        | `..`          | [std::ops::RangeFull]        |            -          |
| _RangeInclusiveExpr_   | start`..=`end | [std::ops::RangeInclusive]   | start &le; x &le; end |
| _RangeToInclusiveExpr_ | `..=`end      | [std::ops::RangeToInclusive] |            x &le; end |

Examples:

```rust
1..2;   // std::ops::Range
3..;    // std::ops::RangeFrom
..4;    // std::ops::RangeTo
..;     // std::ops::RangeFull
5..=6;  // std::ops::RangeInclusive
..=7;   // std::ops::RangeToInclusive
```

r[expr.range.equivalence]
The following expressions are equivalent.

```rust
let x = std::ops::Range {start: 0, end: 10};
let y = 0..10;

assert_eq!(x, y);
```

r[expr.range.for]
Ranges can be used in `for` loops:

```rust
for i in 1..11 {
    println!("{}", i);
}
```
