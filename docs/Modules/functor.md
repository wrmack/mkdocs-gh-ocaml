# Functors
## What is a functor?
!!! quote "Quotes"
    As suggested by the name, a functor is almost like a function. However, while functions are between values, functors are between modules. A functor has a module as a parameter and returns a module as a result. [OCaml.org](https://ocaml.org/docs/functors)
    
    A functor is simply a “function” from modules to modules [OCaml programming](https://cs3110.github.io/textbook/chapters/modules/functors.html?highlight=functor)

!!! example "Example"
    ```ocaml
    module type X = sig
      val x : int
    end
    
    module IncX (M : X) = struct
      let x = M.x + 1
    end;;
    ```
    
    In the above, `IncX` is a **functor** which takes as a parameter a **module** `M` with signature `X`. 
    
    `IncX` has one variable, `x`, which has as its value the `x` value from the input module `M` incremented by 1.
    
    If the above is copied into utop, utop shows the following signatures:
    
    ```ocaml
    module type X = sig val x : int end
    module IncX : functor (M : X) -> sig val x : int end
    ```
    
    The functor, IncX, is defined. 
    
    The type of the input module M has type X. 
    
    M is not defined, it is a placeholder - for a parameter which is a module. The module that is provided for M must comply with the type signature X.
    
    The following defines a module A which complies with type signature X. The value for variable x is set to 0:
    
    ```ocaml
    module A = struct 
      let x = 0 
    end;;
    ```
    A.x is 0. 
    
    If IncX is applied to A it produces a new module:
    
    ```ocaml
    module B = IncX (A);;
    ```
    
    The module returned by applying the functor IncX to module A has the x value of A incremented by 1.
    
    ```ocaml
    utop # A.x;;
    - : int = 0

    utop # B.x;;
    - : int = 1
    ```
