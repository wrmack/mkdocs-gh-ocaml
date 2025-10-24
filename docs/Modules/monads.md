# Monads

A monad is a design pattern. 

A monad takes a type and wraps it in a monad type such that the new monad type holds the original type as well as additional information.

!!! example "Example monads"

    The option monad

    : takes a type (for example an integer 'x') and wraps it in an option type such that if 'x' does not have a value it returns `None` otherwise `Some x`

    The writer monad
    : takes a computation and wraps it with logging information for debugging purposes

    The data monad
    : wraps a type with additional data information which needs to be tracked, rather than using side-effects as in imperative programming 

## Monad signature

A monad has the signature:

```ocaml
  module type Monad = sig
    type 'a t
    val return : 'a -> 'a t
    val bind : 'a t -> ('a -> 'b t) -> 'b t
  end
```
From this signature:

- the type of the monad is defined to be `'a t`. This is a parameterised type, having the constructor type `t` and a parameter `'a` meaning any type.

- the `return` function takes a type `'a` and returns a Monad type`'a t`.

- the `bind` function: 

    - takes 2 parameters:

        - the first parameter has the Monad type `'a t`
        - the second parameter is a function which is applied to `'a`, the unwrapped original first parameter, and returns a type `'b t` (which is compatible with the type of the 'Monad' module but the type parameter can be different to `'a`)
    
    - returns the value of type `'b t`

A monad takes a value, wraps it in a more complex type so that it contains more information, performs a function on the original value and returns a value of the more complex type holding additional information.

In imperative languages, this additional information, which might be some form of state, is tracked outside of functions by using variables. A monad is useful in functional programming for holding additional information while maintaining the functional paradigm.

!!! quote "Quote 1"
    More exactly, a monad can be used where unrestricted access to a value is inappropriate for reasons specific to the scenario. In the case of the Maybe monad, it is because the value may not exist. In the case of the IO monad, it is because the value may not be known yet, such as when the monad represents user input that will only be provided after a prompt is displayed. In all cases the scenarios in which access makes sense are captured by the bind operation defined for the monad; for the Maybe monad a value is bound only if it exists, and for the IO monad a value is bound only after the previous operations in the sequence have been performed. 
    
    [Wikipedia](https://en.wikipedia.org/wiki/Monad_(functional_programming))

!!! quote "Quote 2"
    
    Say I write an evaluator in a pure functional language.
    
    - To add error handling to it, I need to modify each recursive call to check for and handle errors appropriately. Had I used an impure language with exceptions, no such restructuring would be needed.
    - To add a count of operations performed to it, I need to modify each recursive call to pass around such counts appropriately. Had I used an impure language with a global variable that could be incremented, no such restructuring would be needed.
    - To add an execution trace to it, I need to modify each recursive call to pass around such traces appropriately. Had I used an impure language that performed output as a side effect, no such restructuring would be needed.
    
    Or I could use a monad.
    
    [Philip Wadler](https://homepages.inf.ed.ac.uk/wadler/papers/marktoberdorf/baastad.pdf)

## Monad examples
!!! example "Writer monad 1"
    The Writer monad can be used to write logs while evaluating functions.

    The Writer monad has the type `type 'a t = 'a * string`. It takes the original type and wraps it into a tuple comprising the original type and a string.  The string contains the logging information.

    The `Writer.return` function returns the monad type with the string initialised to `""`.

    The `Writer.bind` function takes a value `m`, of monad type (a tuple comprising the original type and a string), extracts the original value, applies the function `f` to the original value to get a result in monad form (a tuple). The function `f` must be a function that returns a tuple of the Writer monad type (a tuple). Finally, `Writer.bind` returns a value of monad type comprising the result and a concatention of the strings.
   
    ```ocaml
    (* The monad signature *)
    module type Monad = sig
      type 'a t
      val return : 'a -> 'a t
      val bind : 'a t -> ('a -> 'b t) -> 'b t
      val ( let* ): 'a t -> ('a -> 'b t) -> 'b t 
    end
    
    (* The function to log written as fun *)
    (fun x -> x + 7)
    
    (* The same function written as a named function *)
    let myfunction x = x + 7
    
    (* The writer monad *)
    module Writer: (Monad with type 'a t = 'a * string) = 
    struct
      type 'a t = 'a * string
      let return x = (x, "")
      let bind m f =
        let (x, s1) = m in
        let (y, s2) = f x in
        (y, s1 ^ s2) 
      let ( let* ) m f = bind m f 
    end
    
    (* The type of the Writer monad is a tuple containing two elements:
       - a value of any type
       - a string for logging *)
       
    (* Using the monad *)
    open Writer
    
    (* Using Writer.bind and fun *)
    bind (10, "Param is 10; ") (fun x -> (x + 7, "Function is x + 7"))
  
    (* Using let* syntax *)
    let* x = (10, "Param is 10; ") in
    (myfunction x, "Function is x + 7")
    
    (* Compare original code with the monad form *)
    
    (* Original *)
    let result =
      let x = 10 in
      myfunction x
      
    (* Using the monad for logging *)
    let result = 
      let* x = (10, "Param is 10; ") in
      (myfunction x, "Function is x + 7")
    
    ```
    The code above more succintly for copying to utop:
    
    ```ocaml

    module type Monad = sig
      type 'a t
      val return : 'a -> 'a t
      val bind : 'a t -> ('a -> 'b t) -> 'b t
      val ( let* ): 'a t -> ('a -> 'b t) -> 'b t 
    end
    
    module Writer: (Monad with type 'a t = 'a * string) = 
      struct
        type 'a t = 'a * string
        let return x = (x, "")
        let bind m f =
          let (x, s1) = m in
          let (y, s2) = f x in
          (y, s1 ^ s2) 
        let ( let* ) m f = bind m f 
      end
      
    open Writer
      
    (* Orginal function *)
    let myfunction x = x + 7;;
    
    (* Logged function *)
    let* x = (10, "first logging phrase; ") in
    (myfunction x, "another logging phrase");;
    
    ```
!!! example "Writer monad 2"

    In this example "bind" is defined slightly differently but has the same signature. It might be easier to see what is going on.

    ```ocaml
    (* The monad signature *)
    module type Monad = sig
      type 'a t
      val return : 'a -> 'a t
      val bind : 'a t -> ('a -> 'b t) -> 'b t
    end

    (* The writer monad *)
    module Writer: (Monad with type 'a t = 'a * string) = 
    struct
      type 'a t = 'a * string
      let return x = (x, "")

      (* The function f must be such that, when it is applied to x, 
        it returns a tuple containing y, the computation result, and the log string. *)
      let bind (x, log1) f = 
        let (y,log2) = f x in
        (y, log1^log2)
    end

    open Writer

    let computation =
      bind (return 3) (fun x ->
        let res = x + 1 in
        bind ((), (Printf.sprintf "Integer: %d\nComputed increment: %d\n" x res)) (fun () ->
          return res
        )
      ) 

    let () =
      let (result, log) = computation in
      Printf.printf "Result: %d\n" (result);
      Printf.printf "Log:\n%s\n" log

    (* 
      It produces:
        Result: 4                         
        Log:
        Integer: 3
        Computed increment: 4
    *)
    ```

## let* syntax
!!! understanding "Understanding let* syntax"
    
    Using the Writer monad example above: 
    
    The function to be logged:
    ```ocaml
    let myfunction x = x + 7;;
    ```
    If a specified value, `10`, was to be passed to `myfunction` for `x` we could write:
    ```ocaml
    let x = 10 in
    myfunction x;;
    ```
    Using the monad and `let*` we write:
    ```ocaml
    let* x = (10, "first logging phrase; ") in
    (myfunction x, "another logging phrase");;
    ```
    Clearly, both examples above are similar except that the second example provides logging.
    
    `let* x` extracts the value of type `'a` from the `'a t` parameter, the function is applied to it and a value of type `'b t` is returned.

## Syntax summary for bind and equivalents

`bind`, `( >>= )` and `( let* )` have the same signature: `'a t -> ('a -> 'b t) -> 'b t`

| You write                            |   Think of as                                           |
| ------------------------------------ |  ------------------------------------------------------ |
| `bind (return x) (fun x -> expr)`    | `bind` takes a wrapped x, and a function. It extracts x, applies the function to x and returns a wrapped result |
| `(return x) >>= (fun x-> expr)`      | Inject x from its wrapped value into the function and return a wrapped result |
| `let* x = (return x) in expr`        | Define x locally from its wrapped value to use in expr, returning a wrapped result  |




