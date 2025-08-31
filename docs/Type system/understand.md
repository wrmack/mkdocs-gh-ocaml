# Understanding types

## Why have types?

Data is held in memory. A compiler needs to have information about how to use what is held in memory. A value has an associated *type* (or *data type*).

!!! quote "Quotes"
    **Type:** something that defines a set of possible values and a set of operations for an object. [C++](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines#S-glossary)

    The *type* of a value defines the interpretation of the memory holding it and the operations that may be performed on the value. [Rust](https://doc.rust-lang.org/reference/types.html)

    **Type:** A collection of values. An estimate of the collection of values that a program fragment can assume during program execution.[Cardelli](http://www.lucacardelli.name/Papers/TypeSystems.pdf)

    If V is the set of all values then a type is a subset of V, with values that have technical properties. The phrase *having a type* is then interpreted as membership in the appropriate set. [Cardelli](http://lucacardelli.name/Papers/OnUnderstanding.A4.pdf) (paraphrased)

    The fundamental purpose of a **type system** is to prevent the occurrence of execution errors during the running of a program. [Cardelli](http://www.lucacardelli.name/Papers/TypeSystems.pdf)
    
    ... a type is defined by specifying three things:
    
    - a name for the type,
    - the values of the type, and
    - the operations that may be performed on values of the type. [Harper](https://www.cs.cmu.edu/~rwh/isml/book.pdf)
    
## Static and dynamic type checking

In simple terms, code is compiled into an executable. The executable is then distributed to users as a program. A user runs the program.

In **static type-checking**, type information is used at **compile time** when compiling the executable. The code will not compile if there are type errors. Type information is discarded once code is compiled. There is no type information at run time.

In **dynamic type-checking**, type information is used at **run time** when the program is run. Because types are checked at run time, dynamically typed code runs slower than statically typed code.

OCaml is statically typed. OCaml code will not compile if there are type errors. Type information is discarded once the executable is compiled. 




