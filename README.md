Command line tool for running Shen repl.

  It loads `~/.shen.shen` initialization file. Which is convenient for
modulesys users. It can execute Shen scripts allowing them to access command
line arguments. Error output is written to stderr.

## Installing

  Copy `config.def.h` into `config.h`. Edit `config.h` to match your
configuration. Type

    make

to build `shen_run`.

## Using

  Your script must have entry function `main` which accepts command line
parameters in one argument of type `(list string)` and returns a boolean
indicating success status. You can run that script either by typing

    shen_run your_script.shen argument1 argument2 ...

Or you can add a line 

    #!/usr/bin/env shen_run

at the first line of your script and make file executable

    chmod +x your_script.shen

Then you can run it as any other program:

    ./your_script.shen argument1 argument2 ...

### Example

    #!/usr/bin/env shen_run

    (define show-args
      [] _ -> _
      [X | R] I -> (let - (output "  ~A: ~S~%" I X)
                      (show-args R (+ I 1))))
    
    (define main
      [] -> (error "Not enough arguments.")
      ["-preved" | _] -> false
      Args -> (let - (output "Arguments~%")
                   - (show-args Args 0)
                (output "End~%")
                true))
