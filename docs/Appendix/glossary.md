# Glossary
[^expr1]:
    [The OCaml Manual](https://ocaml.org/manual/latest/expr.html#ss:expr-basic)
[^expr2]:
    [OCaml programming](https://cs3110.github.io/textbook/chapters/basics/expressions.html)
[^imperative]:
    [Programming in Standard ML](https://www.cs.cmu.edu/~rwh/isml/book.pdf), chapter 2.
[^oper]:
    [The OCaml Manual](https://ocaml.org/manual/latest/expr.html#ss%3Aexpr-operators)
[^param]:
    See Constructed types [The OCaml Manual](https://ocaml.org/manual/latest/types.html)
[^valu1]:
    [The OCaml Manual](https://ocaml.org/manual/latest/expr.html#ss:expr-basic)
[^valu2]:
    [OCaml programming](https://cs3110.github.io/textbook/chapters/basics/expressions.html)
[^valu3]:
    [The OCaml Manual](https://ocaml.org/manual/latest/values.html#start-section)


#### `|`

  : Pipe operator. Used to separate ???
  
#### `'a`

  : A type variable. A placeholder for any type. If it appears multiple times then each occurrence is the same type. A placeholder for a different type could be `'b`.

#### `application binary interface (ABI)`

#### `abstract data type`

  : See [Modules](Modules/modules.md#abstract-data-types) and [Introspection](Type system/introspection.md#abstract-types)

#### `abstract syntax tree (AST)`

#### `abstraction`

#### `algebraic data type (ADT)`
  : See [Understanding types](Type system/understand.md#logic-mathematics-type-theory)

#### `argument`
  : See `parameter`.
  
#### `arity`

#### `assembly language`

#### `association list`

#### `atomic`

#### `BNF`

#### `byte code`

#### `concatenate`

#### `constructor`

#### `concrete data type`

#### `continuation passing style (CPS)`

#### `currying`
  : Breaking down a function with  multiple arguments into multilple sequential functions, each with one argument
  ```ocaml
  let add1 x y = x + y;;
  
  (* Is the same as *)
  let add2 = fun x -> fun y -> x + y;;
  ``` 

#### `DWARF`

#### `ELF`

#### `encapsulation`

#### `evaluate`

#### `expression`

  : An *expression* evaluates to a *value*. In this sense a constant is an expression[^expr1] (`let x = 5`). *"The primary task of computation in a functional language is to evaluate an expression to a value"*[^expr2].
  Also see Rust Chap 8 https://doc.rust-lang.org/reference/statements-and-expressions.html 
  
    **Statements** are instructions that perform some action and do not return a value.
    
    **Expressions** evaluate to a resultant value. 
    
    https://doc.rust-lang.org/book/ch03-03-how-functions-work.html
    
    Note: https://discuss.ocaml.org/t/what-is-the-difference-between-statements-and-expressions-in-ocaml/4525 - everything is an expression; statements return unit, like printing.
    
    Exampleof a statement that performs an action or has a side-effect that does not contribute to the evaluation of an expression is a print statement.
    
    Also good: https://stackoverflow.com/questions/50311629/statements-vs-expressions-in-haskell-ocaml-javascript
    
    What Rust calls a statement (eg in Rust, let x = 6 is a let statement), OCaml calls a definition.
    
    The OCaml manual seems to only use the word 'statement' in relation to 'open ...'

#### `functor`

#### `garbage collector (GC)`

#### `Generalized Algebraic Data Type (GADT)`

  : See [Generlized Algebraic Datatypes](Type system/gadt.md)

#### `heap`

  : see stack

#### `immutable`

#### `imperative programming`
  : Imperative programming comprises executing commands in order to change memory.  Functional programming comprises evaluating expressions. [^imperative]

#### `introspection`

#### `lambda calculus`
  : See [Lambda calculus](Functions/lambda.md)

#### `lexer`

#### `module`

#### `monad`
  : See [monads](monads.md)
  
#### `mutable`

#### `native code`

#### `opacity`, `opaque`

#### `operator`
  : An operator is a symbolic representation of a function application and may be a *prefix* or *infix* operator[^oper].
  ```ocaml
  (* The '+' operator is an infix operator *)
  let sum y z = y + z 
  
  (* Equivalent to *)
  let sum y z  = Int64.add y z
  
  (* Also equivalent to *)
  let sum y z = ( + ) y z
  ```
  
#### `overloading`
  : ??
  
#### `parameter`
  : A function may have one or more **parameters** which are used in the function body.
  
    Concrete **arguments** are passed to the function when it is called.
    ```ocaml
    (* x and y are the parameters of the function named sum *)
    let sum x y = x + y;;
    
    (* 5 and 6 are the arguments passed to sum when it is called *)
    sum 5 6;;
    ```
    A type may have a parameter[^param] (see `parameterised type` below).
    
    A function's parameters are written to the right of the function name.
    
    A type's paramemters are written to the left of the type constructor.
    
#### `parameterised type`
  : See [Parameterised type](Type system/parameterised.md)

#### `parser`

#### `pattern`
  : *"Patterns are templates that allow selecting data structures of a given shape, and binding identifiers to components of the data structure."* [OCaml manual](https://ocaml.org/manual/latest/patterns.html)

    The OCaml manual lists options for such templates, including: value name, constant, "or", variant, polymorphic variant, tuple, record, array, range and other patterns. 
  
#### `polymorphism`

#### `primitive`

#### `stack`

  : Good explanation in Rust https://doc.rust-lang.org/book/ch04-01-what-is-ownership.html
  
    GDB: https://sourceware.org/gdb/wiki/Internals%20All-About-Stack-Frames
  
    - LIFO
    - comprises frames 
    - each frame represents a function call and includes the return address
    - frame includes function variables
    - stack holds data that is known at compile time
    - when a function returns its frame is popped off the stack
    - dynamic data is stored on the heap (requiring garbage collection)
    
  
#### `statement`

  : see Rust
  
#### `strict`

#### `tail recursion`

#### `thread` 

#### `Turing complete, Turing machine`

#### `type`

  : Data is held in memory. A compiler needs to have information about how to use what is held in memory. A value has an associated *type* (or *data type*).
  
    Further info: [Type system](Type system/understand.md)

  
#### `value`

  : An *expression* evaluates to a *value*. In this sense a constant is an expression[^valu1] (`let x = 5`). A value is an expression for which there is no computation remaining to be performed[^valu2]. Values include: integer numbers, floating-point numbers, characters, character strings, tuples, records, arrays, variant values, polymorphic variants, functions, objects[^valu3]. 
  
#### `variable`

#### `variadic function`

  : a function which can take a variable number of paramaters

#### `when``
  : `when`is a keyword used in pattern-matching guards