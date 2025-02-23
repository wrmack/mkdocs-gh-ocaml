# Functions

## Anonymous functions

Sometimes referred to as lambda expressions.

```ocaml
(* Anonymous function with 1 parameter, x *)
fun x -> 2 * x;;

(* Anonymous function with 2 parameters, x & y *)
fun x y -> x + y;;

(* Can be expressed as two functions with single parameters *)
fun x -> (fun y -> x + y);;

(* Applying an anonymous function *)
(fun x -> 2 * x) 5;;        (* Returns 10 *)
(fun x y -> x + y) 6 7;;    (* Returns 13 *)

(* Anonymous function and pattern-matching *)
fun x y -> match x > y with
| true -> (string_of_int x) ^ " is greater than " ^ (string_of_int y)
| false -> (string_of_int x) ^ " is less than or equal to " ^ (string_of_int y)

```

## Named functions

```ocaml
(* Named function with 1 parameter *)
let double = fun x -> 2 * x;;

(* Common syntax *)
let double x = 2 * x;;

(* Named function with 2 parameters *)
let add = fun x y -> x + y;;

(* Common syntax *)
let add x y = x + y;;

(* Applying a named function *)
double 5;;    (* Returns 10 *)
add 6 7;;     (* Returns 13 *)

(* Named function and pattern-matching *)
let compare = fun x y -> match x > y with
  | true -> (string_of_int x) ^ " is greater than " ^ (string_of_int y)
  | false -> (string_of_int x) ^ " is less than or equal to " ^ (string_of_int y)

(* Common syntax *)
let compare x y = match x > y with
| true -> (string_of_int x) ^ " is greater than " ^ (string_of_int y)
| false -> (string_of_int x) ^ " is less than or equal to " ^ (string_of_int y)
```

## `function` keyword
- automatically uses pattern matching
- only takes one parameter

```ocaml
(* Function with a parameter *)
(* Is a special case of pattern matching - there is only one case and it returns an expression *)
function x -> 2 * x;;

(* Applying function *)
(function x -> 2 * x) 5;;

(* Pattern matching *)
(* One pattern tested *)
(function | x -> 2 * x) 5;;
(function x -> 2 * x) 5 ;; (* Same as above *)

(* Two patterns tested - whether the parameter is 0 or some other integer *)
(function | 0 -> "this is zero" | _ -> "this is not zero") 5;;
(function 0 -> "this is zero" | _ -> "this is not zero") 5);; (* Same as above *)

(* Function is bound to a name *)
let myfunction = function 
  | 0 -> "this is zero" 
  | _ -> "this is not zero";;

(* Named function is applied to a parameter *)
myfunction 5;;

```