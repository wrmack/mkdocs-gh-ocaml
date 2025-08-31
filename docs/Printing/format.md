# Format module

!!! reference "References"

    OCaml manual: [Format module](https://ocaml.org/manual/latest/api/Format.html), [Format tutorial](https://ocaml.org/manual/latest/api/Format_tutorial.html)     
    [Format Unraveled](https://hal.science/hal-01503081v1/file/format-unraveled.pdf)    
    [Pretty Printing in OCaml](https://keleshev.com/pretty-printing-in-ocaml-a-format-primer)   
    
    Annotations ([OCaml manual](https://ocaml.org/manual/latest/api/Format.html#fpp))
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
    ## Examples
    These examples are set out for copying into utop. (`;;` is used to tell utop to execute code and should be removed if copying into a .ml file)

    The pattern `fprintf out` is used for the general case and can often be replaced by `printf` if printing to std out.
    !!! example "fprintf"
        ### Basic - using fprintf
        ```ocaml
        let fprintf = Format.fprintf;;
        let out = Format.std_formatter;;
        
        let pp_string out string = fprintf out "%S" string;;
        pp_string out "test";;
        ```
        Explanation:

        - `fprintf` takes a formatter, in this case the formatter for standard output is used

    !!! example "printf"
        ### Basic - using printf
        ```ocaml
        let printf = Format.printf;;
        
        let pp_string string = printf "%S" string;;
        pp_string "test";;
        ```
        Explanation:

        - `printf` assumes the formatter for standard output

    !!! example "The `%a` modifier"
        ### Basic - using the `%a` modifier for inserting a user-printer
        ```ocaml
        let fprintf = Format.fprintf;;
        let out = Format.std_formatter;;

        (* Define a user-printer *)
        let pp_printer out string = fprintf out "%S @." string;;

        (* Use the user-printer in fprintf *)
        let my_print out string = fprintf out "%a @." pp_printer string;;
        
        (* Apply it *)
        my_print out "testing";;
        ```
    
    !!! example "List"
        ### Printing a list
        Iterates through list elements.
        ```ocaml
        let fprintf = Format.fprintf;;
        let out = Format.std_formatter;;
        
        (* Define a user-printer *)
        let print_int_list out ls = 
          fprintf out "[";
          List.iteri
            (fun i x ->
            if i > 0 then fprintf out "; ";
            fprintf out "%d" x ) ls;
          fprintf out "]";;  

        (* Apply it *)
        let () =
          let ls = [1;2;3;4] in
          fprintf out "%a @." print_int_list ls;;
        ```
        This prints:
        ```
        [1; 2; 3; 4]
        ```
        Explanation:

        - the user-printer is the function `print_int_list` which takes a formatter and a list
        - it prints the square bracket "["
        - it iterates through the list and prints each element, denoted by x, in the list
        - prior to printing the element it prints "; " if the index, i, is greater than zero
        - it prints the square bracket "]"

        Using `printf` instead of `fprintf` is a little simpler, but `%a` expects the user-printer to provide a formatter. 
        The user-printer must still use a `fprintf` but the calling function can use `printf`, which passes the standard formatter under the hood:
        ```ocaml        
        (* Define a user-printer *)
        let print_int_list out ls = 
          fprintf out "[";
          List.iteri
            (fun i x ->
            if i > 0 then fprintf out "; ";
            fprintf out "%d" x ) ls;
          fprintf out "]";;  

        (* Apply it *)
        let () =
          let ls = [1;2;3;4] in
          printf "%a @." print_int_list ls;;
        ```        

    !!! example "Format convenience functions"
        ### Printing a list using a Format convenience function
        The convenience function is: Format.pp_print_list.
        ```ocaml
        let fprintf = Format.fprintf;;
        let printf = Format.printf;;
        let out = Format.std_formatter;;
        
        (* Handler for each element of list *)
        let print_int out i = fprintf out "%d" i;;

        (* Define a user-printer *)
        let print_int_list out ls = 
          let sep out () = fprintf out ";" in
          Format.pp_print_list ~pp_sep:sep print_int out ls;;

        let () =
          let ls = [1;2;3;4] in
          printf "[%a] @." print_int_list ls;;
        ```
    !!! example "Table"
        ### Printing a table
        ```ocaml
        (* We want to print this list of tuples in a table
           with each row containing one tuple *)
        open Format;;

        let stats = [("a","b","c");("d","e","f");("g","h","i")];; 

        let mytable statistics =
          open_vbox 0;
          open_tbox ();
          printf "  "; set_tab (); 
          printf "Col 1       "; set_tab (); 
          printf "Col 2       "; set_tab (); 
          printf "Col 3       ";
          let delim = String.init 30 (fun _ -> '-') in
          Format.printf "\n  %s" delim;
          List.iter (fun (x, y, z) -> 
            print_tab ();
            printf "%s" x;
            print_tab ();
            printf "%s" y;
            print_tab ();
            printf "%s" z;    
          ) statistics;
          close_tbox ();
          close_box ();
          print_flush ();
          print_newline ()
          ;;

        let () = mytable stats;;
        ```
        This prints:
        ```
          Col 1       Col 2       Col 3       
          ------------------------------
          a           b           c
          d           e           f
          g           h           i
        ```
        Explanation:

        - open a tabulation box
        - print the table header, setting tabs at same time
        - iterate through the input list of tuples:
            - move to a tab stop then print a tuple element
        - close the tabluation box
        
        Comments:

        - consider [PrintBox](https://github.com/c-cube/printbox) for more features