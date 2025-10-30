# Types, maths and logic

## Category theory

Category theory is a high-level abstraction of mathematics. Simplistically it has two key concepts: objects and arrows. An arrow (representing a morphism) defines the relationship between two objects. A category is a collection of objects and morphisms.

When category theory is applied to programming languages, types can be considered as objects and functions as morphisms (the relationships between types).

!!! reference "References"

    An excellent resource is [Category Theory for Programmers](https://bartoszmilewski.com/2014/10/28/category-theory-for-programmers-the-preface/) by Bartosz Milewski. This has code snippets in C++ and Haskell.  
    
    A PDF OCaml edition is available [here](https://github.com/hmemcpy/milewski-ctfp-pdf/releases). 

    These resources provide an understanding of many functional programming concepts:

    - algebraic data types
    - product types
    - sum types
    - functors
    - monads
  


## Curry-Howard correspondence

There is a correspondence between the language of logic and mathematics and the language of types:    
[Curry-Howard correspondence](https://en.wikipedia.org/wiki/Curry%E2%80%93Howard_correspondence)    
[Type theory](https://en.wikipedia.org/wiki/Type_theory), [Intuitionistic logic](https://en.wikipedia.org/wiki/Type_theory#Connections_to_foundations)    
Fuller explanation of the Curry-Howard correspondance: [OCaml Programming](https://cs3110.github.io/textbook/chapters/adv/curry-howard.html)    
Haskell: [The Curry-Howard isomorphism](https://en.wikibooks.org/wiki/Haskell/The_Curry%E2%80%93Howard_isomorphism)

!!! quote "Wikipedia - [Algebraic data type](https://en.wikipedia.org/wiki/Algebraic_data_type)"
    An **algebraic data type** is a datatype formed from other types.
    
    Two common classes of algebraic types are **product types** (i.e., tuples, and records) and **sum types** (i.e., tagged or disjoint unions, coproduct types or *variant* types).
    
    The values of a **product type** typically contain several values, called fields. All values of that type have the same combination of field types. The set of all possible values of a product type is the set-theoretic product, i.e., the Cartesian product, of the sets of all possible values of its field types.
    
    The values of a **sum type** are typically grouped into several classes, called *variants*. A value of a variant type is usually created with a quasi-functional entity called a *constructor*. Each variant has its own constructor, which takes a specified number of arguments with specified types. The set of all possible values of a sum type is the set-theoretic sum, i.e., the disjoint union, of the sets of all possible values of its variants. Enumerated types are a special case of sum types in which the constructors take no arguments, as exactly one value is defined for each constructor.
    
    Values of algebraic types are analyzed with *pattern matching*, which identifies a value by its constructor or field names and extracts the data it contains. 

!!! quote "[Why I chose OCaml as my primary language](https://xvw.lol/en/articles/why-ocaml.html)"

    **Algebraic types**

    However, in my experience, for a statically typed language to be truly usable, the presence of algebraic types is necessary:

    - **Product types**: These allow grouping values of heterogeneous types (thus creating a conjunction of heterogeneous types). They are generally present in all mainstream languages (for example, objects, which introduce additional concepts, or tuples and records).

    - **Sum types**: These allow constructing a disjunction of heterogeneous value types, with different cases indexed by constructors. While some special cases of sums exist in mainstream languages—like booleans (which are a disjunction of two cases: true and false, i.e., two parameterless constructors) — support for full sum types is often cumbersome in popular languages. For example, Kotlin and Java (and de facto C#) use a construct associated with inheritance relations called sealing. The integration of dedicated sum type syntax also took some time in Scala, which, prior to recent versions, relied on sealed families, making the expression of sums verbose and, in my view, harder to reason about.


!!! understanding "Tuple example"
    - a tuple is a product type and has the type notation `'a * 'b`
    - a tuple is a *cartesian product*
    - a tuple is AND logic
    - a tuple comprising an integer and a string contains values that belong to a logical conjunction of the set of integers and set of strings 
    - a tuple allows grouping values of heterogeneous types

!!! understanding "Variant example"
    - a variant is a sum type
    - a variant is OR logic
    - a variant is sometimes referred to as a 'tagged union' or 'disjoint union'
    - a variant comprises values that belong to a logical disjunction 
    - a variant allows heterogeneous types to be distinguished from one another by means of constructors
    
!!! quote "['Propositions as Types'](https://dl.acm.org/doi/pdf/10.1145/2699407) - Wadler"
    **Propositions as Types** is a notion with depth. It describes a correspondence between a given logic and a given programming language. At the surface, it says that for each proposition in the logic there is a corresponding type in the programming language—and vice versa. Thus we have
    
    *propositions as types*
    
    It goes deeper, in that for each proof of a given proposition, there is a program of the corresponding type—and vice versa. Thus we also have
    
    *proofs as programs*
    
    And it goes deeper still, in that for each way to simplify a proof there is a corresponding way to evaluate a program and vice versa. Thus we further have
    
    *simplification of proofs as evaluation of programs*