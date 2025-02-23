# Format module

!!! reference "References"

    Ocaml manual: [Format module](https://ocaml.org/manual/latest/api/Format.html), [Format tutorial](https://ocaml.org/manual/latest/api/Format_tutorial.html)     
    [Format Unraveled](https://hal.science/hal-01503081v1/file/format-unraveled.pdf)    
    [Pretty Printing in Ocaml](https://keleshev.com/pretty-printing-in-ocaml-a-format-primer)   
    
    Annotations ([Ocaml manual](https://ocaml.org/manual/latest/api/Format.html#fpp))
    ```
    "@["        open a box (open_box 0).
    "@[<v n>"   open a vertical box with an offset of n
    "@]"        close a box (close_box ()).
    "@ "        output a breakable space (print_space ()).
    "@,"        output a break hint (print_cut ()).
    "@;<n m>"   emit a "full" break hint (print_break n m).
                n is width
                m is offset
                If line is broken then offset is added to the indentation of the current
                box else (the value of) width blanks are printed.
    "@."        end the pretty-printing, closing all the boxes still opened (print_newline ()).   
    ```
    Box types
    ```
    h   horizontal 
    v   vertical
    hv  horizontal/vertical 
    b   horizontal-or-vertical, demonstrating indentation
    hov horizontal-or-vertical
    ```
## printf - prints to stdout
!!! example "Examples"
    Use utop for all examples.
    
    Open the Format module. 
    
    ```ocaml
    open Format;;
    ```
    
    #### Format.printf with a conversion specifier
    
    ```ocaml
    printf "This is a test. It prints %s \n%!" "hello world";;
    ```
    Prints:
    ```
    This is a test. It prints hello world 
    ```
    #### Format.printf with pretty-printing annotations
    ```ocaml

    printf "@[<v>This is a test.@ It prints @ %s@]@." "hello world";;
    ```
    Prints:
    ```
    This is a test.
    It prints
    hello world
    ```
    ```ocaml
    printf "@[<v 2>This is a test.@;It prints:@;<0 2>%s@]@." "hello world";;
    ```
    Prints:
    ```
    This is a test.
      It prints:
        hello world

    ```
## fprintf - same as printf but outputs on a formatter

!!! understanding "Understanding"
    `fprintf ff fmt arg1 ... argN` formats the arguments arg1 to argN according to the format string fmt, and outputs the resulting string on the formatter ff ([Reference](https://ocaml.org/manual/latest/api/Format.html#fpp)) 

    In fact `printf` is defined in `format.ml` as `fprintf` outputting to `std_formatter`:
    ```ocaml
    let printf fmt = fprintf std_formatter fmt 
    ```
!!! reference "formatter"
    ```ocaml
    type formatter = {
      (* The pretty-printer scanning stack. *)
      pp_scan_stack : pp_scan_elem Stack.t;
      (* The pretty-printer formatting stack. *)
      pp_format_stack : pp_format_elem Stack.t;
      pp_tbox_stack : tbox Stack.t;
      (* The pretty-printer semantics tag stack. *)
      pp_tag_stack : stag Stack.t;
      pp_mark_stack : stag Stack.t;
      (* Value of right margin. *)
      mutable pp_margin : int;
      (* Minimal space left before margin, when opening a box. *)
      mutable pp_min_space_left : int;
      (* Maximum value of indentation:
        no box can be opened further. *)
      mutable pp_max_indent : int;
      (* Space remaining on the current line. *)
      mutable pp_space_left : int;
      (* Current value of indentation. *)
      mutable pp_current_indent : int;
      (* True when the line has been broken by the pretty-printer. *)
      mutable pp_is_new_line : bool;
      (* Total width of tokens already printed. *)
      mutable pp_left_total : int;
      (* Total width of tokens ever put in queue. *)
      mutable pp_right_total : int;
      (* Current number of open boxes. *)
      mutable pp_curr_depth : int;
      (* Maximum number of boxes which can be simultaneously open. *)
      mutable pp_max_boxes : int;
      (* Ellipsis string. *)
      mutable pp_ellipsis : string;
      (* Output function. *)
      mutable pp_out_string : string -> int -> int -> unit;
      (* Flushing function. *)
      mutable pp_out_flush : unit -> unit;
      (* Output of new lines. *)
      mutable pp_out_newline : unit -> unit;
      (* Output of break hints spaces. *)
      mutable pp_out_spaces : int -> unit;
      (* Output of indentation of new lines. *)
      mutable pp_out_indent : int -> unit;
      (* Are tags printed ? *)
      mutable pp_print_tags : bool;
      (* Are tags marked ? *)
      mutable pp_mark_tags : bool;
      (* Find opening and closing markers of tags. *)
      mutable pp_mark_open_tag : stag -> string;
      mutable pp_mark_close_tag : stag -> string;
      mutable pp_print_open_tag : stag -> unit;
      mutable pp_print_close_tag : stag -> unit;
      (* The pretty-printer queue. *)
      pp_queue : pp_queue;
    }
    ```
    The type `formatter` is defined in the module's `.ml` file but not in its `.mli` (interface) file. In its `.mli` file it is an [abstract type](../../modules/#abstract-data-types):
    ```ocaml
    type formatter
    ```