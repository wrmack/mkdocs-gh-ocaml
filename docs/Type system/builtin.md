# Predefined types
Type definitions that are built into OCaml.

## Primitives 
A primitive cannot be contructed from other types

|Value is     | Type     | Example value      |
|-------------|----------|--------------------|
|an integer   | `int`    | `5`                |
|a float      | `float`  | `5.`               |
|a character  | `char`   | `'c'`              |
|a string     | `string` | `"the"`            |
|a boolean    | `bool`   | `true`<br />`false`|


## Data structures

|Value is  | Type           | Example value | Comment  |
|----------|--------------- |---------------|----------|
|a tuple   | `'a * 'b *'c`<br />`int * string`               | `(anytype, anytype, anytype)`<br />`(12, "the")`   |
|a list    | `'a list`<br />`int list`<br />`string list`    | `[sametype; sametype; sametype]`<br />`[1;2;3]`<br />`["apples";"oranges";"bananas"]` | Size is mutable. Elements are immutable.
|an array  | `'a array`<br />`int array`<br />`string array` | `[|sametype; sametype; sametype |]`<br />`[|1;2;3|]`<br />`[|"apples";"oranges";"bananas"|]`| Size is fixed. Elements are mutable |

!!! understanding "Understanding `int list`"

    The following help to understand `int list`:
    
    - The type of an integer list is neither `int` nor `list` but `int list` 
    - `int` is a type parameter
    - It is a composite type, constructed using the list type constructor, which takes a base type (the primitive, int) and produces a new type (int list).
    - An integer type is wrapped into a list   
    - The type is a list with elements of type `int`
   
