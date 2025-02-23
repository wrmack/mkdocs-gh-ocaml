# Type system
Note: [stackoverflow](https://stackoverflow.com/questions/4200393/signatures-types-in-functional-programming-ocaml)

From "The C++ Programming Language" -by Bjarne Stroustrup the creator of C++.
>A type defines a set of possible values and a set of operations (for an object).

[ref](https://www.geeksforgeeks.org/data-types-in-programming/)
>An attribute that identifies a piece of data and instructs a computer system on how to interpret its value is called a data type. 

[Wikipedia](https://en.wikipedia.org/wiki/Data_type)

There is a universe V of all values, containing simple values like integers,
data structures like pairs, records and variants, and functions. This is a
complete partial order, built using Scott's techniques [Scott 76], but in first
approximation we can think of it as just a large set of all possible computable
values. A type is a set of elements of V. Not all subsets of V are legal types:
they must obey some technical properties. The subsets of V obeying such
properties are called ideals. All the types found in programming languages are
ideals in this sense, so we don't have to worry too much about subsets of V
which are not ideals. Hence, a type is an ideal, which is a set of values.
Moreover, the set of all types (ideals) over V, when ordered by set inclusion,
forms a lattice. The top of this lattice is the type Top (the set of all values,
i.e. V itself). The bottom of the lattice is, essentially, the empty set
(actually, it is the singleton set containing the least element of V). The
phrase having a type is then interpreted as membership in the appropriate set.
As ideals over V may overlap, a value can have many types. The set of types of
any given programming language is generally only a small subset of the set of
all ideals over V. For example any subset of the integers determines an ideal
(and hence a type), and so does the set of all pairs with first element equal to
3. This generality is welcome, because it allows one to accommodate many
different type systems in the same framework. One has to decide exactly which
ideals are to be considered interesting in the context of a particular language.
A particular type system is then a collection of ideals of V, which is usually
identified by giving a language of type expressions and a mapping from type
expressions to ideals. The ideals in this collection are elevated to the rank of
types for a particular language. For example, we can choose the integers,
integer pairs and integer-to-integer functions as our type system. Different
languages will have different type systems, but all these type systems can be
built on top of the domain V (provided that V is rich enough to start with),
using the same techniques. A monomorphic type system is one in which each value
belongs to at most one type (except for the least element of V which, by
definition of ideal, belongs to all types). As types are sets, a value may
belong to many types. A polymorphic type system is one in which large and
interesting collections of values belong to many types. There is also a grey
area of mostly monomorphic and almost polymorphic systems, so the definitions
are left imprecise, but the important point is that the basic model of ideals
over V can explain all these degrees of polymorphism.
http://lucacardelli.name/Papers/OnUnderstanding.A4.pdf

type
<theory, programming>

(Or "data type") A set of values from which a variable, constant, function, or other expression may take its value. A type is a classification of data that tells the compiler or interpreter how the programmer intends to use it. For example, the process and result of adding two variables differs greatly according to whether they are integers, floating point numbers, or strings.
https://foldoc.org/type

The type of a value defines the interpretation of the memory holding it and the operations that may be performed on the value.
[Rust](https://doc.rust-lang.org/reference/types.html)