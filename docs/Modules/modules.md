# Basic modules

## Defining a module
!!! example "Examples"
    ```ocaml
    (* Defining a module type *)
    module type Myarith = sig
      type t
      val myadd: int -> int
      val mysub: int -> int
    end
    ```
    ```ocaml
    (* Defining a module struct *)
    module Myarith = struct
      let myadd x y = x + y
      let mysub x y = x - y
    end
    ```

## Module types
!!! quote "Quotes"

    The implementation details of a module can be hidden by attaching an interface. (Note that in the context of OCaml, the terms ***interface***, ***signature***, and ***module type*** are all used interchangeably.)
    [Real World OCaml](https://dev.realworldocaml.org/files-modules-and-programs.html#signatures-and-abstract-types)

!!! understanding "Understanding"
    The module type is used to control what parts of the module are exposed.
    
    The module struct can have additional declarations in it but if they are not included in the type signature they will not be exposed (for example to utop).
    
!!! example "Examples"
    ```ocaml
    (* Defining a module type using : after the module name *)
    module Mymod: sig
      ...
    end = struct
      ...
    end
    
    (* Defining a module type separately *)
    module type S = sig
      ...
    end
    
    module Mymod: S = struct
      ...
    end
    ```
## Using a module 

### Accessing and using a module in a `.ml` file
[Refer to configuring dune]
#### Use the module name 
Use the module name and dot notation to access its variables and functions
```ocaml
let test = Mymodule.mymodule_var1
```
#### Open the module 
Its variables and functions are available to the current module.
```ocaml
open Mymodule

let test = mymodule_var1
```
#### Open the module locally
It will only be available to the local scope, which is defined by `in expr ` or by `.(...)`
```ocaml
let open Mymodule in ...

(* or *)
Mymodule.(....)
```
#### In utop
```ocaml
#require "mymodule";;
open Mymodule;;
```

## Abstract Data Types

!!! example "Example"
    Longer examples are easier in utop if the code is 
    written in a file which is then used in utop.
    
    The following code is saved in `test.ml`
    ```ocaml
    module Mymod: sig
      type t
      val x: (t * t)
      val add: t -> t -> t
    end = struct
      type t = int
      let x = (1,2)
      let add x y = x + y 
    end
    ```
    In utop:
    
    ```ocaml
    #use "test.ml";;
    open Mymod;;
    
    let z = Mymod.add 1 2;;
    
    ```
    utop does not accept the first argument:
    ```
    Error: This expression has type int but an expression 
    was expected of type t
    ```
    What is the signature of `Mymod.x`?
    
    ```ocaml
    Mymod.x;;
     - : t * t = (<abstr>, <abstr>)
    ```   
    Type `t` is an abstract type. utop does not know it is an integer under the hood.
    
### Reducing the abstraction
    
#### 1\. Change some types.
!!! example "Example"   
    Define Mymod.add to take int arguments. Make the arguments *concrete* types, not *abstract* types.
    
    `test.ml`:
    ```ocaml
    module Mymod: sig
      type t
      val x: (t * t)
      val add: int -> int -> t
    end = struct
      type t = int
      let x = (1,2)
      let add x y = x + y 
    end
    ```
    utop:
    ```ocaml
    #use "test.ml";;
    
    open Mymod;;
    
    let z = Mymod.add 1 2;;
    val z : t = <abstr>
    (* Applying Mymod.add now works but the result is abstract. *)
    ```
    
#### 2\. Include an evaluator.
!!! example "Example"
    Add `int_of_t` to convert the t type to an int.
    
    `test.ml`:
    ```ocaml
    module Mymod: sig
      type t
      val x: (t * t)
      val add: int -> int -> t
      val int_of_t: t -> int
    end = struct
      type t = int
      let x = (1,2)
      let add x y = x + y 
      let int_of_t x = x 
    end
    ```
    utop:
    ```ocaml
    #use "test.ml";;
    open Mymod;;
    
    let z = Mymod.add 1 2;;
    val z : t = <abstr>
    
    Mymod.int_of_t z;;
    - : int = 3
    ```
#### 3\. Add a pretty-printer
!!! example "Examples"
    A simple pretty-printer, using Format.printf.
    
    `test.ml`:
    
    ```ocaml
    module Mymod: sig
      type t
      val x: (t * t)
      val add: int -> int -> t
      val pp: t -> unit 
    end = struct
      type t = int
      let x = (1,2)
      let add x y = x + y 
      let pp z = Format.printf "Pretty printing: %i@." z 
    end
    ```
    utop:
    ```ocaml
    #use "test.ml";;
    open Mymod;;
    #install_printer pp;;
    
    let z = Mymod.add 1 2;;
    Pretty printing: 3
    val z : t = 
    ```
    
    A more complex pretty printer.
    
    `test.ml`
    
    ```ocaml
    module Mymod: sig
      type t
      val x: (t * t)
      val add: int -> int -> t
      val pp_add: t -> unit 
      val pp_show_tuple: (t * t) -> unit
    end = struct
      type t = int
      let x = (1,2)
      let add x y = x + y 
      
      (* Pretty printer for the add function,
         showing how to use a formatter using fprintf *)
      
      (* Define a formatter*)
      let pp_add_ptr out k = 
        Format.fprintf out "Result: %i" k 
        
      (* The pretty printer references the formatter with %a *)
      (* Note it does not have to pass 'out'. Format.printf is 
          fprintf with std_formatter already passed under the hood. *)
      let pp_add z = 
        Format.printf "@[<v>Pretty printing:@;<0 2>%a @]@." pp_add_ptr z 
      
      (* Pretty printer for showing a tuple 
         - it does not utilise a formatter *)
      let pp_show_tuple (a,b) = 
        Format.printf "Showing x: (%i, %i)@." a b
    end
    ```
    utop:
    ```ocaml
    #use "test.ml";;
    open Mymod;;
    
    #install_printer pp_add;;
    #install_printer pp_show_tuple;;
    
    Mymod.x;;
    Showing x: (1, 2)
    - : t * t = 

    Mymod.add 6 7;;
    Pretty printing:
      Result: 13
    ```

    
#### 4\. Use `ppx_deriving.show`
    
    
!!! reference "References"

    [Real World OCaml](https://dev.realworldocaml.org/files-modules-and-programs.html#signatures-and-abstract-types)     
    [Stackoverflow](https://stackoverflow.com/a/48433462)      
    [OCaml Programming - type constraints](https://cs3110.github.io/textbook/chapters/modules/module_type_constraints.html)      
    [OCaml Programming - encapsulation](https://cs3110.github.io/textbook/chapters/modules/encapsulation.html)    
    [How does one print any type?](https://discuss.ocaml.org/t/how-does-one-print-any-type/4362)
    