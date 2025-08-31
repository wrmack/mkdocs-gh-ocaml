# Operations on lists

!!! example "List.fold_left"
    ```ocaml
    (* List.fold_left: takes an operator, an accumulator, a list 
       and returns the accumulator.  *)
       
    (* In the example below: 
       2 + 3 = 5
       5 + 1 = 6
       6 + 5 = 11 
       Returns 11    *)

    List.fold_left (+) 2 [3;1;5]
    
    (* Same as: *)
    List.fold_left (fun x y -> x + y) 2 [3;1;5]
    ```