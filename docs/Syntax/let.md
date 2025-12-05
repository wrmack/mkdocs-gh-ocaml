# let

## let pattern matching

!!! quote "OCaml manual"
    The `let` and `let rec` constructs bind value names locally. The construct

    ```ocaml
    let pattern1 = expr1 and … and patternn = exprn in expr
    ```
    evaluates `expr1 … exprn` in some unspecified order and matches their values against the patterns `pattern1 … patternn`. If the matchings succeed, `expr` is evaluated in the environment enriched by the bindings performed during matching, and the value of `expr` is returned as the value of the whole let expression. If one of the matchings fails, the exception `Match_failure` is raised.


!!! example "Example"
    ```ocaml
    (* Bind a tuple to a name *)
    let tup = (1,'b',"word")
    
    (* The pattern (x,y,z) matches tup *)
    let (x,y,z) = tup
    
    (* Can now access elements of the tuple *)
    let () = Printf.printf "the second element is %c \n%!" y;;
    (* Prints "the second element is b" *)
    
    (* In the expression let () = <print statement> 
       the pattern () stands for unit and matches the 
       print statement *)
    ```

## `let...in`
- `let name = expression` binds an expression to a name
- 'name' cannot be mutated, though it can be redefined (ie with `let name =`)
- `let name = expression1 in expression2` defines the scope of 'name' to be within 'expression2'

!!! example "Examples"
    ```ocaml
    (* Binds an integer to a name 'x' *)
    let x = 5;;
    
    (* Binds a function with two parameters to a name - these are equivalent *)
    let sum x y = x + y;;
    let sum = (fun x y -> x + y);;
    
    (* Binds an expression to a name, to be used in a defined scope *)
    let num = 2 in num * 2;;
    
    (* Another example*)
    let double x = x * 2 in
     let y = double 100 in
     y - 1;;
    
    (* Binds the result of the above to the name 'myresult' *)    
    let myresult = 
     let double x = x * 2 in
     let y = double 100 in
     y - 1;;    
    ```

## let binding operators

!!! quote "[OCaml manual](https://ocaml.org/manual/latest/bindingops.html)"
    Binding operators offer syntactic sugar to expose library functions under (a variant of) the familiar syntax of standard keywords. Currently supported “binding operators” are `let<op>` and `and<op>`, where `<op>` is an operator symbol, for example `and+$`.

    Binding operators were introduced to offer convenient syntax for working with monads and applicative functors; for those, we propose conventions using operators `*` and `+` respectively.
    
!!! understanding "Meaning of let binding operator"

    An operator in OCaml is defined with the syntax:
    ```ocaml
    let ( <op> ) = ...

    (* Example - the + infix operator *)
    let ( + ) = 
    ```
    **let operators**

    The keyword `let` binds an expression to a name.

    A let binding operator provides a special syntax using `let<op>` where `<op>` is an operator symbol like `+` or `*`.

    The syntax uses the `let<op>` operator in a similar way to a an ordinary `let` binding. Hence "let binding operator".

    It is defined like an ordinary operator:

    ```ocaml
    let ( let@ ) = 
    ```

    An `and<op>` operator can be defined.

!!! understanding "Desugaring"
    The let-operator in
    ```ocaml
    let<op> x1 = e1 in e
    ```
    can be desugared into an application
    ```ocaml
    ( let<op> ) e1 (fun x1 -> e)
    ```
    [OCaml manual](https://ocaml.org/manual/latest/bindingops.html#ss%3Aletop-rules)

!!! understanding "Conventions"
    
    By convention:
    
    - applicative functors use `let+`
    - monad functors use `let*`
    
    An applicative functor should provide a module implementing the following interface:
    ```ocaml
    module type Applicative_syntax = sig
      type 'a t
      val ( let+ ) : 'a t -> ('a -> 'b) -> 'b t
      val ( and+ ): 'a t -> 'b t -> ('a * 'b) t
    end
    ```
    where `(let+)` is bound to the map operation and `(and+)` is bound to the monoidal product operation.

    A monad should provide a module implementing the following interface:
    ```ocaml
    module type Monad_syntax = sig
      include Applicative_syntax
      val ( let* ) : 'a t -> ('a -> 'b t) -> 'b t
      val ( and* ): 'a t -> 'b t -> ('a * 'b) t
    end
    ```
    where `(let*)` is bound to the bind operation, and `(and*)` is also bound to the monoidal product operation.

!!! example "`let ( let@ ) x f = f x`"
    ```ocaml
    let (let@) x f = f x;;

    (let@) 10 (fun x -> x + 1);;  (* apply (let@) as a function - returns 11 *)

    let@ x = 10 in x + 1;;        (* use let@ binding operator - reurns 11 *)
    ```


!!! example "`let ( let@ ) f x = f x`"
 
    ```ocaml
    let ( let@ ) f x = f x;;

    let my_wrap fn =
      match fn () with     (* fn () - the function fn is applied to () *)
        | () -> `Ok ()
        | exception e -> `Error (Printexc.to_string e);;

    (* each of the following produce the same result: 
       - not using let@
       - applying (let@) as a function
       - using the let@ operator  *)

    let result = my_wrap (fun () -> Printf.printf "Test1 \n%!");;       

    let result = (let@) my_wrap (fun () -> Printf.printf "Test2 \n%!");; 

    let result =
      let@ () = my_wrap in (Printf.printf "Test3 \n%!");; 

    (* () is the fun parameter 
       if omit 'let result =' need to add ;; in front if not otherwise using ;; *)
    ```



