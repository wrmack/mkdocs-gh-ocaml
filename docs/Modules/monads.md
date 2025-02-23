# Monads

A monad is a design pattern. 

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
## Monad example
!!! example "Writer monad"
    The Writer monad can be used to write logs while evaluating functions.
   
    ```ocaml
    (* The monad signature *)
    module type Monad = sig
      type 'a t
      val return : 'a -> 'a t
      val bind : 'a t -> ('a -> 'b t) -> 'b t
      val ( let* ): 'a t -> ('a -> 'b t) -> 'b t 
    end;;
    
    (* The function to log written as fun *)
    (fun x -> x + 7);;
    
    (* The same function written as a named function *)
    let myfunction x = x + 7;;
    
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
    end;;
    
    (* The type of the Writer monad is a tuple containing two elements:
       - a value of any type
       - a string for logging *)
       
    (* Using the monad *)
    open Writer;;
    
    (* - using Writer.bind and fun *)
    bind (10, "Param is 10; ") (fun x -> (x + 7, "Function is x + 7"));;
  
    (* - using let* syntax *)
    let* x = (10, "Param is 10; ") in
    (myfunction x, "Function is x + 7");;
    
    (* Compare original code with the monad form *)
    
    (* Original *)
    let result =
      let x = 10 in
      myfunction x;;
      
    (* Using the monad for logging *)
    let result = 
      let* x = (10, "Param is 10; ") in
      (myfunction x, "Function is x + 7");;
    
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
