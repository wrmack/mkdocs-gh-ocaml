# Lists
!!! example "Create a list"
    ```ocaml
    let myemptylist = []
    let mylist = [1;2;3;4]
    let mylist = 1::2::3::4::[]
    let mylist = List.cons 1 myemptylist  (* returns [1] *)
    let mylist = List.cons 2 mylist       (* returns [2:1] *)
    let mylist = List.init 5 (fun x -> x) (* returns [0;1;2;3;4] *)
    ```
!!! example "Get an element in a list"
    ```ocaml
    let mylist = [1;2;3;4]
    let hd = List.hd mylist               (* returns 1 *)
    let seconditem = List.nth mylist 1    (* returns 2 *)
    ```
!!! example "Set an element in a list"
    Elements in lists are not mutable. But a list itself can have elements added or removed.