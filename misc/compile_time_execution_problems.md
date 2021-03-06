# Compile Time Execution Problems (Metaprogramming)
2016-11-02

## Memory and Types

Compile time execution (CTE) is a stage of the compiler which runs any Odin code the
user requests before the creation of the executable. The data modified and generated
by this stage will be used as the initialization data for the _compiled_ code.

The CTE stage is an interpreter running the generated _single static assignment_ (SSA)
tree for the requested code. When using the memory generated by the interpreter for the
compiled code, there are a few problems. The main problem being: pointers will point
to invalid memory addresses. This is becaused the memory space of the interpreter is
completely different to the memory space of the executable (compiled code).

The table below presents which data types are safe for transferal and which are not.

Key:

* Y - Yes
* N - No
* D - Dependent on elements
* ? - Highly depends on a lot of factors (most likely no)

| Type      | Safe?                                                                  |
|-----------|------------------------------------------------------------------------|
| boolean   | Y                                                                      |
| integer   | Y                                                                      |
| float     | Y                                                                      |
| pointer   | N - Maybe safe if never changed                                        |
| string    | Y - Even though (ptr+int) interally, still safe to convert to constant |
| any       | N - (ptr+ptr)                                                          |
| array     | D                                                                      |
| vector    | Y - Elements can only be boolean, integer, or float (thus safe)        |
| slice     | N - Internally (ptr+int+int)                                           |
| maybe     | D                                                                      |
| struct    | D                                                                      |
| enum      | Y                                                                      |
| union     | N - (blob+int)                                                         |
| raw_union | N - ^^^                                                                |
| tuple     | D                                                                      |
| proc      | ? - Need to solve the next problem                                     |


## Calling procedures (external and internal)

If all the procedures are only from within the code itself, i.e. not a loaded pointer,
then it is "safe". However, calling external procedures and passing procedures from the
interpreter to external programs _will_ cause problems as many of the procedures are not
stored in _real_ memory. This causes numerous problems.

**TODO:**

* Look at how other languages solve this problem (e.g. LUA)
* ???
