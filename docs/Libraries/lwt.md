# Lwt
This library provides concurrency in OCaml. The key concept is a *promise*.

Description - from the lwt package:
>A promise is a value that may become determined in the future.
             Lwt provides typed, composable promises. Promises that are resolved by I/O are
             resolved by Lwt in parallel.
             Meanwhile, OCaml code, including code creating and waiting on promises, runs in
             a single thread by default. This reduces the need for locks or other
             synchronization primitives. Code can be run in parallel on an opt-in basis.


## utop
If using utop to run examples, tell it to use the lwt package.

!!! example "Example"
    ```ocaml
    #require "lwt";;
    ```

## Promise
- a value that is permitted to mutate once
- does not block program flow
- like an empty box waiting to be filled
- once it is created it is *pending*
- when it is *fulfilled*, or *rejected*, it is *resolved* 
- for example, a function waiting for user input from the terminal

>- Promises of type `'a Lwt.t` are placeholders for values of type `'a`.
>- `Lwt.bind` attaches callbacks to promises. When a promise gets a value, its callbacks are called.
>- Separate resolvers of type `'a Lwt.u` are used to write values into promises, through `Lwt.wakeup_later`.
>- Promises and resolvers are created in pairs using `Lwt.wait`. Lwt I/O functions call `Lwt.wait` internally, but return only the promise.
>- `Lwt_main.run` is used to wait on one “top-level” promise. When that promise gets a value, the program terminates.

### State
A promise is a memory cell that is always in one of three states:

  - *fulfilled*, and containing one value of type 'a,
  - *rejected*, and containing one exception, or
  - *pending*, in which case it may become fulfilled or rejected later.

### Create a promise

#### Fulfilled with the given value.      

`Lwt.return : 'a -> 'a Lwt.t`


#### Rejected with the given exception. 

` Lwt.fail : exn -> 'a Lwt.t`

#### Pending with resolver
Create a pending promise, and return it, paired with a resolver (of type 'a Lwt.u), which must be used to resolve (fulfill or reject) the promise.

`Lwt.wait : unit -> 'a Lwt.t * 'a Lwt.u`

### Resolve a pending promise
#### Fulfill the promise with a value
`Lwt.wakeup : 'a Lwt.u -> 'a -> unit`      

#### Reject the promise with an exception
`Lwt.wakeup_exn : 'a Lwt.u -> exn -> unit`       


!!! example "Example"
    ```ocaml
    let (p : int Lwt.t), r = Lwt.wait ();;
    Lwt.wakeup_later r 42;;
    
    ``` 
### Bind a promise to a function - `Lwt.bind`
>`bind p f` creates a promise which waits for `p` to become become fulfilled, then passes the resulting value to `f`. If `p` is a pending promise, then `bind p f` will be a pending promise too, until `p` is resolved. If `p` is rejected, then the resulting promise will be rejected with the same exception. 

`f` is a *callback* function

!!! example "Example"
    ```ocaml
    Lwt.bind
      (Lwt_io.read_line Lwt_io.stdin)
      (fun str -> Lwt_io.printlf "You typed %S" str)
    ```
### Running an Lwt program - `Lwt_main.run`

>This function waits for the given promise to resolve and returns its result. In fact it does more than that; it also runs the scheduler which is responsible for making asynchronous computations progress when events are received from the outside world.

!!! example "Example"
    In utop: `#require "lwt.unix";;`
    ```ocaml
    let () = Lwt_main.run (Lwt_io.printl "Hello, world!");;
    ```
### Syntactic sugar for bind - `let%lwt` and `let*`

!!! example "Examples"
    In utop: `#require "lwt_ppx";;`
    
    ```ocaml
    (* Without syntactic sugar *)
    let () =
      Lwt_main.run begin
        Lwt.bind Lwt_io.(read_line stdin) (fun line ->
          Lwt.bind (Lwt_unix.sleep 1.) (fun () ->
            Lwt_io.printf "One second ago, you entered %s\n" line))
      end;;
    
    (* Using let%lwt - requires lwt_ppx *)
    let () = 
      Lwt_main.run begin
        let%lwt line = Lwt_io.(read_line stdin) in              (* promise *)
        let%lwt () = Lwt_unix.sleep 1. in                       (* promise *)
        Lwt_io.printf "One second ago, you entered %s\n" line   (* callback *)
      end;;
      
    (* Using let* *)
    let ( let* ) = Lwt.bind;;
    let () = 
      Lwt_main.run begin
        let* line = Lwt_io.(read_line stdin) in                 (* promise *)
        let* () = Lwt_unix.sleep 1. in                          (* promise *)
        Lwt_io.printf "One second ago, you entered %s\n" line   (* callback *)
      end;;      
    ```
