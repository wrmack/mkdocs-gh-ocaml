[^fn1]:
    Also see [Richard Feynman's Learning Technique](https://fs.blog/feynman-learning-technique/)


# Introduction

## Why these notes
There is some excellent documentation aleady available for Ocaml:

- the official [manual](https://ocaml.org/manual/latest/index.html)
- Ocaml website [learning resources](https://ocaml.org/docs)
- [Real World Ocaml](https://dev.realworldocaml.org)
- Ocaml programming [textbook](https://cs3110.github.io/textbook/cover.html).

However, I only dabble in Ocaml from time so: 

- I need a quick reference that is more than a cheat sheet
- I like the adage: to learn something, teach it (in this case, write up some documentation)[^fn1] 

These notes reflect personal reactions to learning Ocaml:

- prefer examples to signatures (*"don't show me the function signature - show me how I use it"*).
- what does ... mean? When learning, one browses various information sources, such as Stackoverflow or the Ocaml Discord. People refer to advanced concepts and terminology whose meaning is too hard to look up using Google. I have tried to provide a basic glossary to cover this.

## Information in boxes

These notes use the following box types throughout:

!!! example "Examples"
    Code examples
    
!!! quote "Quotes"
    Helpful authoritative quotes

!!! understanding "Understanding"
    Additional comments to aid understanding
    
!!! reference "References"
    Links to sources
    
## Other interesting documentation

!!! reference "Related languages"
    It can help to understand concepts in Ocaml by reading the documentation of related languages
    
    [F#](https://fsharp.org/docs/)
    :  Derived from Ocaml and used with the .NET framework
    
    [Haskell](https://www.haskell.org/documentation/)
    :  A functional programming language
    
    [Rust](https://www.rust-lang.org/learn)
    :  A newer language which [acknowledges influences from Ocaml](https://doc.rust-lang.org/reference/influences.html)
    
    [SML](https://smlfamily.github.io) (Standard ML)
    :  Describes Ocaml as a "close cousin".  Has an interesting [history page](https://smlfamily.github.io/history/index.html). The book [Programming in Standard ML](https://www.cs.cmu.edu/~rwh/isml/book.pdf) has very good explanations.

