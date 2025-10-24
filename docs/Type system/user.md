# User-defined types

The type is defined by the user.

## Records

A record is a collection of key / value pairs.

A record type is defined:

```ocaml
type [name] = {key1:type; key2:type; key3:type}
```

!!! example "Examples"
    ```ocaml
    type car = {maker:string; model:string; year:int};; 
    let mycar = {maker = "Mazda"; model = "CX-5"; year = 2020};;
    ```
    
## Variants

A variant value is one of alternatives. Each alternative is separated by the pipe operator.

A variant type is defined:
```ocaml
type [name] = [constructor] | [constructor] ...
```

!!! example "Examples"
    ```ocaml
    (* Enumerated constants *)
    type fruit = Apple | Pear | Banana;;
    let myfruit = Apple;;
    
    (* Built-in option type *)
    type 'a option = Some of 'a | None;;
    
    (* Built-in list type *)
    type 'a list = [] | ( :: ) of 'a * 'a list;;
    
    type colour =
      | Red | Green | Blue | Yellow | Black | White
      | RGB of {r : int; g : int; b : int};;
    let mycolour = RGB {r = 125; g = 50; b = 255};;  
    ```


