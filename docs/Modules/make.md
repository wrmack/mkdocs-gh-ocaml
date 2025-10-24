# The 'Make' pattern

A functor is a module which is applied to another module and returns a module. A name commonly given to certain functors is `Make`. Such a module is used to construct another module.

In the standard library:

- `Set.Make` is a functor building an implementation of the set structure
- `Map.Make` is a functor building an implementation of the map structure
- `Hashtbl.Make` is a functor building an implementation of the hashtable structure.

The pattern: `module Make () = struct ... end`  takes unit, `()`, as its parameter.  In other words there is no external input. It can be used for creating different instances of a module.