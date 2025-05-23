r[type.tuple]
# Tuple types

r[type.tuple.syntax]
```grammar,types
TupleType ->
      `(` `)`
    | `(` ( Type `,` )+ Type? `)`
```

r[type.tuple.intro]
*Tuple types* are a family of structural types[^1] for heterogeneous lists of other types.

The syntax for a tuple type is a parenthesized, comma-separated list of types.

r[type.tuple.restriction]
1-ary tuples require a comma after their element type to be disambiguated with a [parenthesized type].

r[type.tuple.field-number]
A tuple type has a number of fields equal to the length of the list of types.
This number of fields determines the *arity* of the tuple.
A tuple with `n` fields is called an *n-ary tuple*.
For example, a tuple with 2 fields is a 2-ary tuple.

r[type.tuple.field-name]
Fields of tuples are named using increasing numeric names matching their position in the list of types.
The first field is `0`.
The second field is `1`.
And so on.
The type of each field is the type of the same position in the tuple's list of types.

r[type.tuple.unit]
For convenience and historical reasons, the tuple type with no fields (`()`) is often called *unit* or *the unit type*.
Its one value is also called *unit* or *the unit value*.

Some examples of tuple types:

* `()` (unit)
* `(i32,)` (1-ary tuple)
* `(f64, f64)`
* `(String, i32)`
* `(i32, String)` (different type from the previous example)
* `(i32, f64, Vec<String>, Option<bool>)`

r[type.tuple.constructor]
Values of this type are constructed using a [tuple expression].
Furthermore, various expressions will produce the unit value if there is no other meaningful value for it to evaluate to.

r[type.tuple.access]
Tuple fields can be accessed by either a [tuple index expression] or [pattern matching].

[^1]: Structural types are always equivalent if their internal types are equivalent.
      For a nominal version of tuples, see [tuple structs].

[parenthesized type]: ../types.md#parenthesized-types
[pattern matching]: ../patterns.md#tuple-patterns
[tuple expression]: ../expressions/tuple-expr.md#tuple-expressions
[tuple index expression]: ../expressions/tuple-expr.md#tuple-indexing-expressions
[tuple structs]: ./struct.md
