# opam
opam is OCaml's package manager.

## opam help
In a terminal:
```bash
opam --help
opam <subcommand> --help
```
## List packages
!!! example "Example"
    In a terminal:
    ```bash
    # List installed packages
    opam list
    
    # List all available packages
    opam list -a
    ```
## Search for a package
!!! example "Example"
    In a terminal:
    
    ```bash
    # Search for all packages mentioning 'lwt'
    opam list --search lwt
    opam search lwt
    ```
## Show information about a package
!!! example "Example"
    In a terminal:
    ```bash
    # Show information about the lwt package
    opam show lwt
    
    # Show the files installed by lwt
    opam show --list-files lwt
    ```
## Switches
Packages are installed in switches. These are independent installations of the ocaml compiler 
and the packages you have installed in that switch. If you want to try a new compiler version
but don't want to upset dependencies of your current set of installed packages then you can create
a new switch based on the new compiler.
!!! example "Example"
    In a terminal:
    
    ```bash
    # Help for switches
    opam switch --help
    
    # Show installed switches
    opam switch list
    # or just:
    opam switch
    
    # Show available ocaml compiler versions
    opam list ocaml
    
    # Create a new switch using ocaml compiler version "4.14.1"
    opam switch create 4.14.1   
    # or, giving it a label:
    opam switch create "myswitch" 4.14.1
    
    # Set a switch, from the available switches, for the current environment
    opam switch set 4.14.1
    # then do:
    eval $(opam env)
    ```