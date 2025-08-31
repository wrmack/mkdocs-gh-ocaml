# Example 1: `cmdliner`

`cmdliner` is a library for wrapping a CLI command in additional information relating to its documentation. When the executable is run with `--help` it displays a man (manual) page. If the executable is run without the appropriate arguments a short help will be displayed.

!!! reference "References"
    cmdliner [tutorial, documentation and api](https://erratique.ch/software/cmdliner/doc/)    
    cmdliner [source code](https://github.com/dbuenzli/cmdliner)
    
The following is an example of a command-line tool `chorus` that is provided in the tutorial.

The tool will take a message `MSG` and print it `COUNT` times. The help for the tool  will look like:

```bash
chorus [-c COUNT | --count=COUNT] [MSG]
```
!!! quote "From the [tutorial](https://erratique.ch/software/cmdliner/doc/tutorial.html)"
    With **Cmdliner** your tool's **main** function evaluates a command.
    
    A command is a value of type `Cmdliner.Cmd.t` which gathers a command name and a term of type `Cmdliner.Term.t`. A term is an expression to be evaluated. The type parameter of the term (and the command) indicates the type of the result of the evaluation.
    
    One way to create terms is by lifting regular OCaml values with `Cmdliner.Term.const`. Terms can be applied to terms evaluating to functional values with `Cmdliner.Term.($)`.

!!! example "chorus.ml"

    ```ocaml linenums="1"

    (* The chorus tool *)
    let chorus count msg = 
      for i = 1 to count do 
        print_endline msg 
      done

    open Cmdliner    

    (* The tool's arguments *)
    
    let count =
      let doc = "Repeat the message $(docv) times." in
      Arg.(value & opt int 10 & info ["c"; "count"] ~docv:"COUNT" ~doc)
    
    let msg =
      let env =
        let doc = "Overrides the default message to print." in
        Cmd.Env.info "CHORUS_MSG" ~doc
      in
      let doc = "The message to print." in
      Arg.(value & pos 0 string "Revolt!" & info [] ~env ~docv:"MSG" ~doc)
    
    (* chorus is applied to count & msg with each being of type Term.t *)
    let chorus_t = Term.(const chorus $ count $ msg)  
    
    (* The command that is evaluated by Cmd.eval in the main function *)
    let cmd =
      let doc = "print a customizable message repeatedly" in
      let man = [
        `S Manpage.s_bugs;
        `P "Email bug reports to <bugs@example.org>." ]
      in
      let info = Cmd.info "chorus" ~version:"%‌%VERSION%%" ~doc ~man in
      Cmd.v info chorus_t
    
    (* The main function *)
    let main () = exit (Cmd.eval cmd)
    let () = main ()
    ```
    
Copying and pasting each value above into utop show the following signatures:

```ocaml
val chorus : int -> string -> unit = <fun>
val count : int Term.t = <abstr>
val msg : string Term.t = <abstr>
val chorus_t : unit Term.t = <abstr>
val cmd : unit Cmd.t = <abstr>
val main : unit -> 'a = <fun>
```
These signatures tell us:

- the tool "chorus" is a function that takes an integer (the `count` parameter) and a string (the `msg` parameter) and returns unit.

- the `count` value is an abstract value having type `int Term.t`. According to the tutorial quoted above when it is evaluated it provides a type equivalent to the type's parameter - an `int`.

- the `msg` value is an abstract value having type `string Term.t`. According to the tutorial quoted above when it is evaluated it provides a type equivalent to the type's parameter - a `string`.

The line `let chorus_t = Term.(const chorus $ count $ msg)` needs picking apart.

- `Term.(body)` means the module `Term` is opened locally in the body between the brackets. 

- `Term.const` has type `'a -> 'a t`.  It takes a type `'a` and returns a type `'a t`. In other words it lifts the value with type `'a` into the Term context. Possibly the identifier 'const' is short for constructor or constraint? It takes a value of type `'a` and constructs a value of type `'a t` (or constrains it to that type).

- `chorus` is a function which takes two parameters, an `int` and a `string`.

- `Term.const chorus $ count $ msg` has the signature of an [applicative functor](../../Modules/applicative.md)
  - the function `chorus` is lifted into the Term context by `Term.const` and applied to `count` (already type Term.t)
  - the result is applied to `msg` (already type Term.t)
  - it returns type `unit Term.t` (plain chorus returns type `unit`)

