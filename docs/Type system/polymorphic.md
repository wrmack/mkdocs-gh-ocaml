# Polymorphic variants

- Can use them without declaring them first
- They are anonymous types
- Constructors ('tags') start with a back tick

!!! example "Examples"
    ```ocaml
    let plus x y = `Plus (x,y)
    match plus 5 7 with | `Plus (a,b) -> a + b (* returns: 12 *);;
    ```