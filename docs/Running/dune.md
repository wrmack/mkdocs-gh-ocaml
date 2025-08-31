!!! reference "References"
    [Getting started with OCaml](https://lambdafoo.com/posts/2021-10-29-getting-started-with-ocaml.html)
    
!!! example "Recipes"
    #### Create a project
    ```bash
    # Create a folder, myproject, containing a project skeleton
    dune init proj myproject

    # Move into the root folder of the project to work on it
    cd myproject
    ```
    #### Build the project
    Create a folder, `_build`, containing the compiled executable.
    ```bash
    dune build
    ```
    
    #### Run the executable
    ```bash
    dune exec myproj
    ```

    #### Clean the project
    ```bash
    dune clean
    ```
