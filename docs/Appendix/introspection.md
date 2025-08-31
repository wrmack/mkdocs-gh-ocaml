# Inspecting code in utop and at runtime

## utop

In utop

!!! example "Examples"
    Display the signature of a module: `#show_module modulename`.
    
    ```ocaml
    #show_module List;;
    
    open Format;;
    #show_module Format;;
    ```
## Abstract types

See [modules](../modules.md) for examples of reducing abstraction.

!!! understanding "Brief explanation"
    A module can hide the definition of a type.
    
    The signature for Mymod below does not define type t.  It is defined in the struct.
    
    The signature for the `add` function uses the t type.
    
    The following works because the `add` function is called within the struct:
    
    ```ocaml
    module Mymod: sig
      type t
      val add: t -> t -> t
    end = struct
      type t = int
      let add x y = x + y 
      let () = Printf.printf "Using add function: %i\n%!" (add 2 3)
    end
    ```
    The following gives an error because it expects type `t`.
    
    In utop
    
    ```ocaml
    open Mymod;;
    
    let sum = Mymod.add 3 2;;
    
    (* Error: This expression has type int but an expression was expected of type
             Mymod.t *)
    ```
    
    