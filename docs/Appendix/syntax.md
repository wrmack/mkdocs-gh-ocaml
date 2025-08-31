# Syntax summary

A quick reference.

## let
`let` binds a value to a name. The value bound to the name cannot be mutated. The name is an *identifier* and is not a variable.

!!! example "Example"
    ```ocaml
    let x = 42 
    let f x = 2 * x
    ```

    These are definitions. Definitions bind values to names. In the first case the value 42 is being bound to the name x. 
    In the second case, a function definition, the function `2 * x` is being bound to the name f.

## let...in
`let...in` binds a value to a name locally. The name is not accessible globally.

!!! example "Example"
    ```ocaml
    let x = 2 in
    let y = x + 3
    ```
    
## list

!!! example "Examples"
    ```ocaml
    let mylist1 = [1;2;3]
    let mylist2 = 0::mylist1
    mylist2 @ [4]   (* returns [0; 1; 2; 3; 4] *)
    ```
## tuple

!!! example "Examples"
    ```ocaml
    let mytuple = (1,"alpha",mylist1)
    ```
## record

!!! example "Examples"
    ```ocaml
    type myrecord = {first:string; second:int}
    let record1 = {first = "alpha"; second = 2}
    record1.first   (* returns "alpha" *)

    let record2 = {record1 with second = 3}
    record2   (* returns {first = "alpha"; second = 3} *)   
    ```
## array

!!! example "Examples"
    ```ocaml
    let myarray = [| 1; 2; 3; 4; 5 |]
    myarray.(2)    (* returns 3 *)
    ```