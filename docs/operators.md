# Operators

## Operators represent functions

!!! example "Examples"
    The `+` operator is defined to be the function `Int.add`
    ```ocaml
    (* Defining the operator '+' *)
    let ( + ) = Int.add;;

    5 + 7;;
    
    (* ...is equivalent to: *)
    ( + ) 5 7;;
    
    (* ...is equivalent to: *)
    Int.add 5 7;;

    ```
    
    The `::` operator is a list constructor defined in a variant type.
    
    ```ocaml
    type 'a list = [] | ( :: ) of 'a * 'a list;;
    
    (* As an infix operator *)
    1 :: 2 :: 3 :: [];;
    
    (* Applying :: as a variant constructor *)
    ( :: ) (1,[2;3]);;
    ```
    
## Infix and prefix operators
- an infix operator is placed between two arguments
- a prefix operator is placed in front of an argument

!!! example Example
    The `+` operator is an infix operator
    ```ocaml
    x + y
    ```
    The `!` operator is a prefix operator
    ```ocaml
    !x
    ```
    
## Built-in operators
```
Operator	Description
  =	        Structural equality[1]
 <>	        Structural inequality[1]
  <	        Less than
  >	        Greater than
 <=	        Less than or equal
 >=	        Greater than or equal
 ==	        Physical equality (same object)[1]
 !=	        Physical inequality (not same object)[1]
 &&	        Boolean and
  &	        (Deprecated) Boolean and
 ||	        Boolean or
  |	        (Deprecated) Boolean or
 |>	        Reverse function application (x |> f is the same as f x)
 @@	        Function application (f @@ x is the same as f x)
 ~-	        Integer negation (same as unary -)
 ~+	        Described as "unary addition" but doesn't seem to do anything.
  +	        Integer addition
  -	        Integer subtraction
  *	        Integer multiplication
  /	        Integer division
~-.	        Float negation (same as unary -.)
~+.	        Described as "unary addition" but doesn't seem to do anything.
 +.	        Float addition
 -.	        Float subtraction
 *.	        Float multiplication
 /.	        Float division
**	        Float exponentiation
 ^	        String concatenation
 @	        List concatenation
 !	        Get the value of a ref
:=	        Set the value of a ref
^^	        Format string concatenation
```
## Composition operators

!!! example "Examples"
    A function composition operator can be used to avoid using nested brackets.
    ```ocaml
    (* Three functions *)
    let f x = x + 1
    let g x = x + 2
    let h x = x + 3;;
    
    (* Function composition using nested brackets *)
    h (g (f 5));;

    (* Using the built-in @@ infix composition operator *)
    h @@ g @@ f 5;;
    
    (* Defining an infix composition operator *)
    let ( & ) f v = f v;;
    h & g & f 5;;
    
    ```
!!! reference "References"
    [Operator cheatsheet](https://www.brendanlong.com/ocaml-operator-cheatsheet.html)
