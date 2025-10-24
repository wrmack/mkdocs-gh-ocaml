# Lambda calculus

!!! quote "Wikipedia [Lambda calculus](https://en.wikipedia.org/wiki/Lambda_calculus)"
    Lambda calculus has played an important role in the development of the theory of programming languages. Functional programming languages implement lambda calculus.

Lambda calculus is composed of 3 elements: variables, functions, and applications.

|Name	|Syntax	|Example	|Explanation|
|-----|-------|---------|-----------|
|Variable	|&lt;name&gt;	|x	|a variable named "x"|
|Function	|λ&lt;parameters&gt;.&lt;body&gt;	|λx.x	|a function with parameter "x" and body "x"
|Application	| &lt;function&gt;&lt;variable or function&gt;	|(λx.x)a	|calling the function "λx.x" with argument "a"

!!! example "Examples"
    ```ocaml
    lambda calculus      OCaml equivalent  evaluates to
    
    λx.x                 (fun x -> x)
    (λx.x) a             (fun x -> x) a     a
    ```
!!! reference "References"
    Wikipedia [lambda calculus](https://en.wikipedia.org/wiki/Lambda_calculus)    
    Learn X in Y minutes [Lambda calculus](https://learnxinyminutes.com/lambda-calculus/)    
    Compared to SML [CS 312 Recitation 26](https://www.cs.cornell.edu/courses/cs3110/2008fa/recitations/rec26.html)