# Universal toplevel for OCaml (utop)

!!! reference "References"
    A blog [post](https://opam.ocaml.org/blog/about-utop/) on opam.ocaml.org    
    Github utop [README](https://github.com/ocaml-community/utop?tab=readme-ov-file#readme)    
    [OCaml Programming](https://cs3110.github.io/textbook/chapters/basics/toplevel.html)     
    OCaml manual: [Top level](https://ocaml.org/manual/latest/toplevel.html)
    
    


## Getting started with utop
###Install
In a terminal:
```bash
opam install utop
```
### Help
In a terminal:
```bash
man utop
utop --help
```
In utop itself:
```ocaml
#help
```
### Start utop
In a terminal:
```shell
utop
```
## Using utop
### Ending phrases
Place a double semicolon ;; at the end of a phrase to tell utop to evaluate and print the result of the given phrase.
It will not evaluate the phrase otherwise.

### Completion bar
The completion bar at the bottom shows possible completions as you type.
!!! note
    ```pre
    M-left, M-right:  to move along the completion bar      
    M-down:           to select a word
    ```
    (The M key is usually the alt key)
### Directives
Directives are instructions to utop and not part of the code. Directives begin with `#`.
!!! example "Example"
    In utop:
    ```ocaml
    (* help *)
    #help;;
    
    (* help using utop *)
    #utop_help;;
    
    (* key bindings *)
    #utop_bindings;;
    
    (* require a package *)
    #require "package name";;
    ```
## Packages, libraries and modules
#### Use opam to find packages, list installed packages or show a package.
!!! example "Using opam"
    We know we have the package `lwt` installed - what should utop `#require` and what are the module names to `open`?
    
    In a terminal:
    ```bash
    opam show --list-files lwt
    
    ...
    ../.opam/4.14.1/lib/lwt/unix/lwt_gc.ml
    ../.opam/4.14.1/lib/lwt/unix/lwt_gc.mli
    ../.opam/4.14.1/lib/lwt/unix/lwt_io.cmi
    ../.opam/4.14.1/lib/lwt/unix/lwt_io.cmt
    ../.opam/4.14.1/lib/lwt/unix/lwt_io.cmti
    ../.opam/4.14.1/lib/lwt/unix/lwt_io.cmx
    ../.opam/4.14.1/lib/lwt/unix/lwt_io.ml
    ../.opam/4.14.1/lib/lwt/unix/lwt_io.mli
    ../.opam/4.14.1/lib/lwt/unix/lwt_main.cmi
    ../.opam/4.14.1/lib/lwt/unix/lwt_main.cmt
    ../.opam/4.14.1/lib/lwt/unix/lwt_main.cmti
    ...
    ```
    As usual, module names are file names capitalised.
    
    This shows there is a module `Lwt_io` which can be accessed in utop by requiring the package "lwt.unix"
    
    To show the signatures of all the types and functions provided by the Lwt_io module, in utop:
    ```ocaml
    #require "lwt.unix";;
    #show Lwt_io;;
    ```
    To open the module `Lwt_io` in utop:

    ```ocaml
    #require "lwt.unix";;
    open Lwt_io;;
    ```
#### Use dune to show installed libraries
!!! example "Using dune"
    ```bash
    dune installed-libraries
    
    ...
    lwt                                   (version: 5.9.1)
    lwt.unix                              (version: 5.9.1)
    lwt_react                             (version: 1.2.0)
    ...
    ```