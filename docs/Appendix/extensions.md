# Filename extensions

## Source code files
`.ml`
  : OCaml implementation code

`.mli`
  : OCaml interface

## Compiled code files



!!! reference "References"
    [Real World OCaml](https://dev.realworldocaml.org/compiler-backend.html#summarizing-the-file-extensions)    
    OCaml Manual [ocamlc](https://ocaml.org/manual/5.3/comp.html)    
    OCaml Manual [ocamlopt](https://ocaml.org/manual/5.3/native.html)    


We’ve seen how the compiler uses intermediate files to store various stages of the compilation toolchain. Here’s a cheat sheet of all them in one place.   

.ml are source files for compilation unit module implementations.
.mli are source files for compilation unit module interfaces. If missing, generated from the .ml file.
.cmi are compiled module interface from a corresponding .mli source file.
.cmo are compiled bytecode object file of the module implementation.
.cma are a library of bytecode object files packed into a single file.
.o are C source files that have been compiled into native object files by the system cc.
.cmt are the typed abstract syntax tree for module implementations.
.cmti are the typed abstract syntax tree for module interfaces.
.annot are old-style annotation file for displaying typed, superseded by cmt files.
The native code compiler also generates some additional files.

.o are compiled native object files of the module implementation.
.cmx contains extra information for linking and cross-module optimization of the object file.
.cmxa and .a are libraries of cmx and o units, stored in the cmxa and a files respectively. These files are always needed together.
.S or .s are the assembly language output if -S is specified.