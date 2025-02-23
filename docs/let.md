# let

## let pattern matching

!!! quote "Ocaml manual"
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

!!! quote "[Ocaml manual](https://ocaml.org/manual/latest/bindingops.html)"
    Binding operators offer syntactic sugar to expose library functions under (a variant of) the familiar syntax of standard keywords. Currently supported “binding operators” are `let<op>` and `and<op>`, where `<op>` is an operator symbol, for example `and+$`.

    Binding operators were introduced to offer convenient syntax for working with monads and applicative functors; for those, we propose conventions using operators `*` and `+` respectively.
    
!!! example "`let*`"
    This is a monadic binding operator.
    ```ocaml
    let* p_1 = e_1 in e_2
    
    (* ——— is equivalent to: ———————————————————————————————— *)
    
    ( let* ) e_1 (fun p_1 -> e_2)
    ```
    
!!! example "`let+`"
    This is a map binding operator.
    ```ocaml
    let+ p_1 = e_1 in e_2
    
    (* ——— is equivalent to: ———————————————————————————————— *)
    
    ( let+ ) e_1 (fun p_1 -> e_2)
    ```