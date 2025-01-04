#course_cs50 

- Bugs are omnipresent even in industry.
- One of the tools in your toolkit is `printf` or any logging function.

# C's debugger

- The other is the debugger that is built into most languages.
- Access by running `debug50` (CS50's wrapper around the usual C debugger) on the compiled script, then place a breakpoint in your source script in VSCode. You can then access the debugger by running `debug50 ./program_name`.
- The code needs to be compiled already; so this won't help you with syntax errors but is more a tool for logical errors.
- The debugger shows your variables, a Watch section, and the Call Stack.
    - Note that when you first initialise a variable but don't assign a value to it, it will contain a *garbage value*. Think of this as what was in that memory slot before it was assigned to this variable (it's more complicated than that though).
- We can:
    - Run the program to the end
    - Step over the current line of code and execute it
    - Step into this line of code and look into the function
    - Step out of the process
    - Restart the process
    - Or end the process