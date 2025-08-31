# The OCaml manual notation

The official [OCaml manual](https://ocaml.org/manual) uses BNF-like notation.

## What is BNF?

!!! understanding "BNF (Backus Naur Form) notation" 
    `terminal symbol`
      : - cannot be replaced
        - enclosed in quotes `"..."`
    
    `non-terminal symbol`
      : - can be replaced
        - enclosed in angle brackets `<...>`
    
    ` <symbol> ::= __expression__`
      : Meaning:
      
        - the non-terminal symbol on the left is replaced by the expression
        - is referred to as a "rule"
    
    `|`&emsp;&emsp;&emsp;&emsp;&ensp;indicates a choice
    
    `+`&emsp;&emsp;&emsp;&emsp;&ensp;one or more of the previous
    
    `*`&emsp;&emsp;&emsp;&emsp;&ensp;zero or more of the previous
      
    `?`&emsp;&emsp;&emsp;&emsp;&ensp;zero or one occurances of the previous
    
    `[...]`&emsp;&emsp;optional
      
    `{...}`&emsp;&emsp;repetition
    
    `(...)`&emsp;&emsp;grouping
      
    `(*...*)`&ensp;&thinsp;comment
    
    <!-- &emsp; is the width of m; &ensp; is the width of n -->
    
    [Wikipedia](https://en.wikipedia.org/wiki/Backus%E2%80%93Naur_form)     
    [BNF playground](https://bnfplayground.pauliankline.com)

## OCaml manual BNF-like notation

!!! quote "From the [OCaml manual](https://ocaml.org/manual/latest/language.html)"
    Terminal symbols are set in typewriter font (`like this`). Non-terminal symbols are set in italic font (*like that*). Square brackets `[…]` denote optional components. Curly brackets `{…}` denotes zero, one or several repetitions of the enclosed components. Curly brackets with a trailing plus sign `{…}+` denote one or several repetitions of the enclosed components. Parentheses `(…)` denote grouping.
  
!!! example "Examples"
    
    *ident*	::=	(*letter* | _ ){*letter* | 0…9 | _ | '}
    
    *letter*	::=	A…Z | a…z
    
    **Meaning**
    
    An identifier (*ident*) commences with a *letter* or underscore. That is followed by zero or more: letters, numerals, underscores or '.
    
    A *letter* is any uppercase or lowercase alphabetical literal.