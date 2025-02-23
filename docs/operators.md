# Operators

## Operators represent functions

!!! example "Examples"
    The `+` operator is defined to be the function `Int.add`
    ```ocaml
    (* Defining the operator '+' *)
    let ( + ) = Int.add;;

    5 + 7;;
    
    (* ...is equivalent to: *)
    ( + ) 5 7;;
    
    (* ...is equivalent to: *)
    Int.add 5 7;;

    ```
    
    The `::` operator is a list constructor defined in a variant type.
    
    ```ocaml
    type 'a list = [] | ( :: ) of 'a * 'a list;;
    
    (* As an infix operator *)
    1 :: 2 :: 3 :: [];;
    
    (* Applying :: as a variant constructor *)
    ( :: ) (1,[2;3]);;
    ```
    
## Infix and prefix operators
- an infix operator is placed between two arguments
- a prefix operator is placed in front of an argument

!!! example Example
    The `+` operator is an infix operator
    ```ocaml
    x + y
    ```
    The `!` operator is a prefix operator
    
## Built-in operators

## Composition operators

!!! example "Examples"
    A function composition operator can be used to avoid using nested brackets.
    ```ocaml
    (* Three functions *)
    let f x = x + 1
    let g x = x + 2
    let h x = x + 3;;
    
    (* Function composition using nested brackets *)
    h (g (f 5));;

    (* Using the built-in @@ infix composition operator *)
    h @@ g @@ f 5;;
    
    (* Defining an infix composition operator *)
    let ( & ) f v = f v;;
    h & g & f 5;;
    
    ```
