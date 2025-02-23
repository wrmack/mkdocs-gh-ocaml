# Lists
#### Equivalent ways of constructing a list
```ocaml
let mylist = [1;2;3;4];;
let myotherlist = 1::2::3::4::[];;
```
#### Accessing a list item using the List module in Stdlib
```ocaml
let seconditem = List.nth mylist1 1;;
```
