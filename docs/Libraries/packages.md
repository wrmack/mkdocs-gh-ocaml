# Modules, libraries and packages

Modules can be defined by *compilation units* (`.ml` and `.mli`files) and module definitions in code.

Libraries are collections of modules.

A package is a distribution unit for code, meta data and build instructions to be managed by a package manager like `opam`. A package might contain libraries, executables, documentation and metadata. 

!!! quote "[dune](https://dune.readthedocs.io/en/stable/overview.html#terminology)"

    package
    : A set of libraries and executables that opam builds and installs as one.