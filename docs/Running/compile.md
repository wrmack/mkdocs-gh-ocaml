# Compiling

## Bytecode
- an instruction set designed for execution by an interpreter (Wikipedia)
- the OCaml interpreter is ocamlrun
- compiled by `ocamlc`
- [Ref](https://caml.inria.fr/pub/docs/manual-ocaml/comp.html), [Ref](http://dev.realworldocaml.org/compiler-backend.html)

**Arguments used by ocamlc** - a good description in ocamlfind [docs](http://projects.camlcity.org/projects/dl/findlib-1.8.1/doc/guide-html/x89.html)

| Argument  | Description  |
|------|-------|
|.mli  | Source files for compilation unit interfaces. From the file x.mli, the ocamlc compiler produces a compiled interface in the file x.cmi |
|.ml   | Source files for compilation unit implementations. From the file x.ml, the ocamlc compiler produces compiled object bytecode in the file x.cmo |
|.cmo  | Compiled object bytecode. These files are linked together, along with the object files obtained by compiling .ml arguments (if any), and the OCaml standard library, to produce a standalone executable program. |
|.cma  | Libraries of object bytecode or plugin bytecode(loaded at rutime). A library packs in a single file a set of object bytecode files (.cmo files) |
|-o exec-file |The name of the output file produced by the compiler (default: a.out under Unix and camlprog.exe under Windows) 


**Output**

- If `a.out` is the name of the file produced by the linking phase, the command `ocamlrun a.out arg1 arg2 … argn` executes the compiled code contained in `a.out`
- On most systems, the file produced by the linking phase can be run directly, as in: `./a.out arg1 arg2 … argn`

## Native code
- also called machine code
- machine language which is executed directly on computer's CPU
- therefore hardware-CPU dependent 
- compiled by `ocamlopt`
- [Ref](https://caml.inria.fr/pub/docs/manual-ocaml/native.html), [Ref](http://dev.realworldocaml.org/compiler-backend.html), [Ref](https://ocaml.org/learn/tutorials/compiling_ocaml_projects.html)

**Arguments used by ocamlopt**

| Argument  | Description  |
|------|-------|
|.mli |Source files for compilation unit interfaces. From the file x.mli, the ocamlopt compiler produces a compiled interface in the file x.cmi. |
|.ml  |Source files for compilation unit implementations. From the file x.ml, the ocamlopt compiler produces: x.o, containing native object code, and x.cmx, containing extra information for linking and optimization. |  
|.cmx  |compiled object code     |  
|.cmxa  |libraries of object code |
|.cmxs  |plugin (loaded at runtime|
| -o exec-file |  the name of the output file produced by the linker (default: a.out under Unix and camlprog.exe under Windows) |                       |
|.cmt, cmti | "The compiler is able to emit some information on its internal stages. It can output .cmt files for the implementation of the compilation unit and .cmti for signatures if the option -bin-annot is passed to it (see the description of -bin-annot below). Each such file contains a typed abstract syntax tree (AST), that is produced during the type checking procedure. This tree contains all available information about the location and the specific type of each term in the source file. The AST is partial if type checking was unsuccessful. These .cmt and .cmti files are typically useful for code inspection tools." [Ref](https://caml.inria.fr/pub/docs/manual-ocaml/native.html)

**Output**

- The output of the linking phase is a regular Unix or Windows executable file. It does not need ocamlrun to run.

!!! reference "References"
    [Compiling to Assembly from Scratch](https://keleshev.com/compiling-to-assembly-from-scratch/#table-of-contents)