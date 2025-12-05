# Lists

`list` is a module in the standard library.

Its type is [defined](https://github.com/ocaml/ocaml/blob/trunk/stdlib/list.ml):

```ocaml
type 'a t = 'a list = [] | (::) of 'a * 'a list
```
It has two constructors: `[]` the empty list and `(::) of 'a * 'a list`. 

`[]` is a special [constant](https://ocaml.org/manual/5.2/const.html#start-section)

`::` is a special constructor which is used for [patterns](https://ocaml.org/manual/5.2/patterns.html#start-section)