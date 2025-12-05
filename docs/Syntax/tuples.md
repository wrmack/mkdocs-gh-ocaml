# Tuples

!!! example "Access an element"
    ```ocaml
    let mytuple = (1,"alpha",['a';'b';'c'])
    
    (* Method 1 *)
    let get_elem2 (_,el,_) = el
    get_elem2 mytuple
    
    (* Method 2 *)
    (fun (x,y,z) -> y) mytuple

    (* Method 3 *)
    match mytuple with (x,y,z) -> y
    
    ```