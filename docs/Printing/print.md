# Simple printing

!!! example "Examples"
    In utop:
    ```ocaml
    print_endline "a string";;
    print_char 'c';;
    print_string "a string";;
    print_int 5;;
    print_float 2.5;;
    ```
## Printf module

### Conversion specification
See the full conversion specification in the [manual](https://ocaml.org/manual/latest/api/Printf.html).

!!! example "Examples"
    Formatting and printing to standard output
    ```ocaml
    Printf.printf "There are %i %s \n%!" 15 "apples";; 
    (* Prints: There are 15 apples *)
    
    let x = 20;;
    Printf.printf "There are %i %s \n%!" x "bananas";;
    (* Prints: There are 20 bananas *)
    
    Printf.printf "%#016x \n%!" 255;; 
    Printf.printf "%#016x \n%!" 0xff;;
    (* Each of these prints: 0x000000000000ff *)
    (* Explanation:
       #: 0x prefix 
       0: padding
      16: width 
       x: hexadecimal
      \n: line
      %!: flush buffer *)
    ```
    Returning a formatted string rather than printing it using sprintf
    ```ocaml
    let x = 20;;
    let mystring = Printf.sprintf "There are %i bananas \n%!" x;;
    ```
### Escape sequences
https://ocaml.org/manual/latest/lex.html

| Sequence	| Character denoted |
|-----------|-------------------|
| \\	      | backslash (\)     |
| \"	      | double quote (")  |
| \'	      | single quote (')  |
| \n	      | linefeed (LF)     |
| \r	      | carriage return (CR)  |
| \t	      | horizontal tabulation (TAB)  |
| \b	      | backspace (BS)               |
| \space	  | space (SPC)                  |
| \ddd	    | the character with ASCII code ddd in decimal    |
| \xhh	    | the character with ASCII code hh in hexadecimal |
| \oooo	    | the character with ASCII code ooo in octal      |