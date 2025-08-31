# Syntax and style guide

!!! reference "References"
    Jane Street [style guide](https://opensource.janestreet.com/standards/)     
    [OCaml Best Practices for Developers](https://wiki.xenproject.org/wiki/OCaml_Best_Practices_for_Developers)

## Names - upper / lower case
!!! example "Examples"
    ```ocaml
    (* Identifiers: snake_case not camelCase *)
    let my_identifier = "mystring";;
    
    (* Variant names: Upper case *)
    type myvariant = | First of int | Second of int
    
    (* Module names: Upper case*)
    module Mymodule = struct
      ...
    end
      
    ```
  

## ; - semicolon
A semicolon separates statements.

Otherwise lines in OCaml code do not end in a semi-colon.

!!! example "Example"

    ```ocaml
    print_int 5;
    print_string "things";
    print_endline "here"
    ```
## ;; - double semi-colon

Used in utop to tell utop to evaluate.
Not needed at end of each line.
A phrase needs to be evaluated if its result is used in successive phrases.
    
!!! example "Example"
    In utop:
    ```ocaml
    (* In the code below the module Printf is opened so 
    we can use printf instead of having to write Printf.printf *)
    
    (* The only place that needs a ;; is at the end to tell utop to evaluate *)
    open Printf
    let myconst = 5
    let () = printf "myconst is %i \n%!" myconst
    let () = print_endline "the end";;
    
    (* Sometimes if utop gives a syntax error message it may 
       be due to a missed ;; - utop may need to evaluate 
       preceding expressions before proceeding further *)
    ```
