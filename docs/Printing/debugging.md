# Using print statements when debugging

## Inserting into code
!!! example "Examples"

    ```ocaml
    (* Open the Printf module *)
    open Printf
    ```
    ```ocaml
    (* Insert a print statement at the top level of the module's code *)
    let () = printf "got to here\n"

    (* Insert a print statement into a local scope *)
    ... in
    let () = printf "got to here\n" in
    ...
    ```
    ```ocaml
    (* Print inside an if-else statement *)
    let a = 0

    let x =  (* original x *)
      if a = 0 then 5
      else 6    

    let x = (* using printf to see if branch gets called *)
      if a = 0 then let () = printf "here\n" in 5
      else 6
    ```
## Printing a list
!!! example "Examples"

    ```ocaml
    open Printf                             (* or open Format *)

    (* Printer for an integer list *)       
    let pp_int_list fmt ls =                (* By convention pretty-printers are prefixed with pp_ *)
      fprintf fmt "[";                      (* Print the opening bracket *)
      List.iteri (fun i x ->                (* Iterate through the list using the index and the value of each list element *)
        if i > 0 then fprintf fmt "; ";     (* Before printing the next list element print a semi-colon and a space *)
        fprintf fmt "%d" x) ls;             (* Print the list's integer element *)
      fprintf fmt "]"                       (* Print the closing bracket *)

    let my_test_list = [1;2;3;4;5]
    let () = printf "my_test_list: %a@." pp_int_list my_test_list

    (* The above prints: *)
    my_test_list: [1; 2; 3; 4; 5] 
    ```
## Printing a table from a list of string lists
!!! example "Examples"
    
    ```ocaml
    open Format 

    (* Printer for printing a table from a string list *)
    let pp_table fmt ls =
      print_flush ();
      open_vbox 0;
      open_tbox ();
      fprintf fmt "  "; set_tab (); 
      printf  "First name     "; set_tab (); 
      printf  "Second name    "; set_tab (); 
      printf  "Address       "; set_tab (); 
      printf  "Phone         "; set_tab (); 
      printf  "Email    "; 
      let delim = String.init 65 (fun _ -> '-') in
      printf "\n  %s" delim;
      List.iter (fun item -> 
        match item with
        | [a;b;c;d;e] ->
          print_tab ();
          printf "%s" a;
          print_tab ();
          printf "%s" b;
          print_tab ();
          printf "%s" c; 
          print_tab ();
          printf "%s" d;    
          print_tab ();
          printf "%s" e;
        | _ -> printf "\nError\n";  
      ) ls;
      close_tbox ();
      close_box ();
      print_flush ()

    (* This is the list *)
    let mylist = 
      [["FirstName-0"; "SecondName-0"; "Address-0"; "Phone-0"; "Email-0"]; ["FirstName-1"; "SecondName-1"; "Address-1"; "Phone-1"; "Email-1"]; ["FirstName-2"; "SecondName-2"; "Address-2"; "Phone-2"; "Email-2"]; ["FirstName-3"; "SecondName-3"; "Address-3"; "Phone-3"; "Email-3"]; ["FirstName-4"; "SecondName-4"; "Address-4"; "Phone-4"; "Email-4"]; ["FirstName-5"; "SecondName-5"; "Address-5"; "Phone-5"; "Email-5"]; ["FirstName-6"; "SecondName-6"; "Address-6"; "Phone-6"; "Email-6"]; ["FirstName-7"; "SecondName-7"; "Address-7"; "Phone-7"; "Email-7"]; ["FirstName-8"; "SecondName-8"; "Address-8"; "Phone-8"; "Email-8"]; ["FirstName-9"; "SecondName-9"; "Address-9"; "Phone-9"; "Email-9"]]
    
    (* Print the list using the pp_table pretty-printer *)
    let () = printf "%a@." pp_table mylist

    ```
    
    This prints:

    ```

      First name     Second name    Address       Phone         Email
      -----------------------------------------------------------------
      FirstName-0    SecondName-0   Address-0     Phone-0       Email-0
      FirstName-1    SecondName-1   Address-1     Phone-1       Email-1
      FirstName-2    SecondName-2   Address-2     Phone-2       Email-2
      FirstName-3    SecondName-3   Address-3     Phone-3       Email-3
      FirstName-4    SecondName-4   Address-4     Phone-4       Email-4
      FirstName-5    SecondName-5   Address-5     Phone-5       Email-5
      FirstName-6    SecondName-6   Address-6     Phone-6       Email-6
      FirstName-7    SecondName-7   Address-7     Phone-7       Email-7
      FirstName-8    SecondName-8   Address-8     Phone-8       Email-8
      FirstName-9    SecondName-9   Address-9     Phone-9       Email-9

    ```