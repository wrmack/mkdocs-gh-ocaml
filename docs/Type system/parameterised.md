# Parameterised types

!!! quote "[Ocaml manual](https://ocaml.org/manual/latest/typedecl.html#start-section)"

    The optional type parameters are either one type variable `'ident`, for type constructors with one parameter, or a list of type variables (`'ident1,…,'identn`), for type constructors with several parameters. Each type parameter may be prefixed by a variance constraint `+` (resp. `-`) indicating that the parameter is covariant (resp. contravariant), and an injectivity annotation `!` indicating that the parameter can be deduced from the whole type. 

!!! example "Examples"

    A parameterised type:
    ```ocaml
    type int List
    type 'a List
    type 'a t
    ```
    Two variables as parameters:
    ```ocaml
    type ('a, 'b) disj = Left of 'a | Right of 'b
    ```