# Applicative functors

## A use case for an applicative functor
It is easy enough to add two numbers:
```ocaml
let myadd x y = x + y
```
If the numbers are each wrapped into an Option type then addition is more difficult.
```ocaml
myadd (Some 3) (Some 4);;
```
utop says:
```ocaml
Error: This expression has type 'a option
       but an expression was expected of type int
```
An applicative functor to the rescue ....

!!! quote "ChatGPT"
    Prompt: Explain applicative functor in OCaml
    
    ChatGPT:
    
    An applicative functor is a structure that allows you to apply functions wrapped in a context to values also wrapped in a context. 
    etc ....
## Applicative functor signature
An applicative has the signature:

```ocaml
module type FUNCTOR = sig
  type 'a t
  val map : ('a -> 'b) -> 'a t -> 'b t
end;;

module type APPLICATIVE = sig
  include FUNCTOR
  val pure : 'a -> 'a t
  val apply : ('a -> 'b) t -> 'a t -> 'b t
end;;
```
## Applicative functor examples
!!! example "OptionApplicative"
    ### First define a module: OptionApplicative
    ```ocaml
    module type FUNCTOR = sig
      type 'a t
      val map : ('a -> 'b) -> 'a t -> 'b t
    end;;
    
    module type APPLICATIVE = sig
      include FUNCTOR
      val pure : 'a -> 'a t
      val apply : ('a -> 'b) t -> 'a t -> 'b t
    end;;

    module OptionApplicative : APPLICATIVE with type 'a t = 'a option = struct
      type 'a t = 'a option
    
      let pure x = Some x
    
      let apply f_opt x_opt =
        match f_opt, x_opt with
        | Some f, Some x -> Some (f x)
        | _ -> None
    
      let map f x_opt =
        match x_opt with
        | Some x -> Some (f x)
        | None -> None
    end;;
    ```
    
    ### Examine OptionApplicative
    Copy the definition above into utop then open it in utop.
    ```ocaml
    open OptionApplicative;;
    ```
    #### The `pure` function
    The `pure` function lifts an ordinary type (in this case `int`) into the OptionApplicative type (`int t` which is equal to `int option`):
    ```ocaml
    let z = pure 6;;
    val z : int t = Some 6
    ```
    `z` has type `int t` and the value of `z` is `Some 6`
    
    #### The `apply` function
    This function is defined as:
    ```ocaml
    let apply f_opt x_opt =
      match f_opt, x_opt with
      | Some f, Some x -> Some (f x)
      | _ -> None
    ```
    Pattern-matching shows that in the case that `f_opt` matches the pattern `Some f` and `x_opt` matches the pattern `Some x` 
    then the `apply` function will return `Some (f x)`. `(f x)` indicates `f` is a function that is applied to `x`.
    
    The signature of the `apply` function is:
    ```ocaml
    #show apply;;
    val apply : ('a -> 'b) t -> 'a t -> 'b t
    ```
    This means the first parameter of `apply` has type `('a -> 'b) t` which is a function of type `('a -> b')` which has been converted to type `('a -> b') t`.
    This function `('a -> b')` takes one parameter of type `'a` and returns a value of type `'b`. A function of type `('a -> 'b)` can be converted to
    a function of type `('a -> 'b) t` using the `pure` function.
    
    #### Incrementing an integer parameter of option type
    The function `fun x -> x + 1` takes an integer parameter and returns a value equal to the parameter incremented by 1.

    ```ocaml
    apply (pure (fun x -> x + 1)) (Some 3);;
    - : int t = Some 7
    ```
    
    #### Adding two integers of option types
    A function that adds two integers takes two parameters:
    
    ```ocaml
    let myadd x y = x + y
    ```
    
    This can be split into two functions, each taking one parameter. It can be partially applied.
    
    ```ocaml
    let myadd = (fun x -> (fun y -> x + y));;
    
    let z = myadd 5 6;;          (* apply myadd to two parameters  - returns the sum *)
    val z : int = 11
    
    let z = myadd 5;;            (* apply myadd to only one parameter - returns a function taking one parameter *)
    val z : int -> int = <fun>
    
    z 6;;
    - : int = 11                 (* apply z to one parameter *)
    ```
    In a similar fashion `apply` can be applied to a function and two parameters:
    
    ```ocaml
    let myoptadd = apply (apply (pure (fun x -> (fun y -> x + y))) (Some 3)) (Some 4);;
    val myoptadd : int t = Some 7
    ```
    An alternative way to write this:
    
    ```ocaml
    let myoptadd1 = (pure (fun x -> (fun y -> x + y)))
    let myoptadd = apply (apply myoptadd1 (Some 3)) (Some 4);;
    ```
    #### The infix operator `<*>`
    
    ```ocaml
    let ( <*> ) = apply
    ```
    Instead of writing `apply fx x` we write `fx <*> x`.
   
    Using the infix operator `+` as a function `( + )`, these are equivalent:
    
    ```ocaml
    let z1 = apply (pure (+)) (Some 3);;
    let z2 = (pure (+)) <*> (Some 3);;
    ```
    And these are equivalent:
    ```ocaml
    let sum1 = apply y (Some 4);;
    let sum2 = y <*> (Some 4);;
    ```
## References
!!! reference "References"
    [Applicative programming with effects](https://www.researchgate.net/publication/215446169_Applicative_Programming_with_Effects)    
    [More type classes in OCaml](https://blog.shaynefletcher.org/2017/05/more-type-classes-in-ocaml.html)
## Grok
!!! quote "Grok"
    In OCaml, an **applicative functor** is a concept from functional programming that generalizes the idea of a functor (something that can be mapped over, like a list or option) by adding the ability to apply functions wrapped in the functor context to values also wrapped in that same context. It’s a powerful abstraction that sits between functors and monads in terms of expressiveness.
    
    Let me break it down step-by-step and provide an explanation with OCaml examples.
    
    ### Definition
    An applicative functor is a type constructor (like `'a option` or `'a list`) equipped with two key operations:
    
    1. **`pure`**: A way to lift a value into the functor context.
    
        - Signature: `'a -> 'f 'a`
        - Example: For `option`, this would turn a value `x` into `Some x`.
    2. **`apply` (or `<*>)`**: A way to apply a function wrapped in the functor to a value wrapped in the same functor.
    
        - Signature: `'f ('a -> 'b) -> 'f 'a -> 'f 'b`
        - Example: Apply `Some (fun x -> x + 1)` to `Some 5` to get `Some 6`.
    
    In OCaml, we typically define these operations in a module that represents the applicative functor.
    
    ### Example: Option as an Applicative Functor
    The `option` type is a classic example of an applicative functor. Here's how we might define it:
    
    ```ocaml
    module OptionApplicative = struct
      type 'a t = 'a option
    
      (* Lift a value into the option context *)
      let pure x = Some x
    
      (* Apply a wrapped function to a wrapped value *)
      let apply fo xo =
        match fo, xo with
        | Some f, Some x -> Some (f x)
        | _ -> None
    end
    ```
    
    - **`pure`**: Takes any value `x` and wraps it in `Some`, placing it in the `option` context.
    - **`apply`**: Takes a function wrapped in an `option` (e.g., `Some (fun x -> x + 1)`) and a value wrapped in an `option` (e.g., `Some 5`), and applies the function to the value if both are `Some`. If either is `None`, the result is `None`.
    
    #### Usage
    ```ocaml
    let f = OptionApplicative.pure (fun x -> x + 1)  (* Some (fun x -> x + 1) *)
    let x = OptionApplicative.pure 5                (* Some 5 *)
    let result = OptionApplicative.apply f x        (* Some 6 *)
    
    let f2 = OptionApplicative.pure (+)
    let result2 = OptionApplicative.apply (OptionApplicative.apply f2 (Some 3)) (Some 4)  (* Some 7 *)
    ```
    
    In the second example, we use `apply` twice to handle a binary function (`+`), applying it to `Some 3` and `Some 4` to get `Some 7`.
    
    ### Properties of Applicative Functors
    Applicative functors must satisfy certain laws (though we don’t enforce them in code):
    
    1. **Identity**: `apply (pure id) x = x`
        - Applying the identity function wrapped in `pure` does nothing.
    2. **Composition**: `apply (apply (pure (fun x y -> x (y z))) f) g x = apply f (apply g x)`
        - Function composition works as expected.
    3. **Homomorphism**: `apply (pure f) (pure x) = pure (f x)`
        - Applying a pure function to a pure value is the same as applying the function and then lifting the result.
    4. **Interchange**: `apply f (pure x) = apply (pure (fun g -> g x)) f`
        - The order of lifting and applying can be swapped in a specific way.
    
    These laws ensure that the applicative functor behaves predictably.
    
    ### Another Example: List as an Applicative Functor
    The `list` type can also be an applicative functor, but there are two common interpretations:
    
    1. **Zip-like behavior**: Pair up elements positionally (less common).
    2. **Cartesian product**: Apply the function to all combinations (more common).
    
    Here’s the Cartesian product version:
    
    ```ocaml
    module ListApplicative = struct
      type 'a t = 'a list
    
      let pure x = [x]
    
      let apply fs xs =
        List.concat_map (fun f -> List.map (fun x -> f x) xs) fs
    end
    ```
    
    #### Usage
    ```ocaml
    let fs = ListApplicative.pure (fun x -> x + 1)  (* [fun x -> x + 1] *)
    let xs = [1; 2; 3]
    let result = ListApplicative.apply fs xs        (* [2; 3; 4] *)
    
    let fs2 = [((+) 1); ((+) 2)]
    let xs2 = [10; 20]
    let result2 = ListApplicative.apply fs2 xs2     (* [11; 21; 12; 22] *)
    ```
    
    Here, `apply` applies every function in the `fs` list to every value in the `xs` list, producing all possible combinations.
    
    ### Comparison to Functors and Monads
    - **Functor**: Only provides `map` (e.g., `List.map`, `Option.map`), which applies a regular function `'a -> 'b` to a wrapped value `'f 'a` to get `'f 'b`.
    - **Applicative Functor**: Adds `pure` and `apply`, allowing functions wrapped in the context (`'f ('a -> 'b)`) to be used.
    - **Monad**: Even more powerful, with `bind` (or `>>=`), which allows the computation to depend on the result of the previous step, unlike applicative functors where operations are independent.
    
    For example, with `option`:
    
    - Functor: `Option.map (fun x -> x + 1) (Some 5)` → `Some 6`
    - Applicative: `OptionApplicative.apply (Some (fun x -> x + 1)) (Some 5)` → `Some 6`
    - Monad: `Option.bind (Some 5) (fun x -> if x > 0 then Some (x + 1) else None)` → `Some 6`
    
    Applicative functors are useful when you want to apply functions in a context without the full power (or complexity) of monads.
    
    ### Conclusion
    In OCaml, an applicative functor is typically implemented as a module with a type `'a t`, a `pure` function to lift values, and an `apply` function to combine wrapped functions and values. It’s a flexible abstraction that works well for types like `option`, `list`, and others, enabling elegant, context-aware function application. Let me know if you’d like more examples or clarification!