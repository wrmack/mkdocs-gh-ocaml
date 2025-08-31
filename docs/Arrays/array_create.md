# Create and access an array

!!! quote "Quotes"
    Arrays are finite, variable-sized sequences of values of the same type. [Reference](https://ocaml.org/manual/5.3/api/Array.html)

    Arrays are fixed-length mutable sequences with constant-time access and update. So they are similar in various ways to refs, lists, and tuples. Like refs, they are mutable. Like lists, they are (finite) sequences. Like tuples, their length is fixed in advance and cannot be resized. [Reference](https://cs3110.github.io/textbook/chapters/mut/arrays.html?highlight=array)

!!! example "Create an array"
    ```ocaml
    let numbers = [|1;2;3;4;5|]
    ```
    ```ocaml
    let arr = Array.make 5 0  (* returns [| 0; 0; 0; 0; 0|] *)
    ```

!!! example "Get an element in an array"
    ```ocaml
    let el = numbers.(2)          (* returns 3 *)
    ```
    ```ocaml
    let el = Array.get numbers 2  (* returns 3 *)
    ```
!!! example "Set an element in an array"
    ```ocaml
    numbers.(2) <- 9              (* changes element 2 in place *)
    ```
    ```ocaml
    Array.set numbers 2 9
    ```