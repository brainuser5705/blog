- function call is an instruction to the CPU
- **caller**: function initiating the function call, **callee/called function**: the function being called
- nested functions not supported
- *metasyntactic variables*: placeholder names for functions or variables
- **return by value**: process of a function returning a value
- `main` returns a **status/exit/return code**
    - C++ standard defines three status codes(0, EXIT_SUCCESS, EXIT_FAILURE)
        - 0 and EXIT_SUCCESS both mean successful execution
        - EXIT_SUCCESS AND EXIT_FAILURE are *preprocessor macros* defined in `<cstdlib>` header
        - use these to maximize portability
- cannot call `main` function explicity, should be at the bottom of the file
- value-returning function that returns no value has undefined behavior
- `main()` implicitly returns a 0 if no return statement is provided
    - best practice is to explicitly return a value
- only one return statement will run
    ```cpp
    int getNumbers(){
        return 5;
        return 7;
    }
    int main(){
        std::cout << getNumbers() << '\n';
        std::cout << getNumbers() << '\n';
    }
    ```
    - value 5 is returned and the function exits immediately, the second return statement never executes
- if you forget the parentheses in the function call, it will cause undefined behavior

# Non-value returning (void) function
- no return statement is required
    - it can be used `return;` but is redundant at the end of a functoin since the return will happen anyway
- cannot use void function where a value is required
- return value from a void function is as compile error
- **early returns**: cause function to return caller before the end of the function is reached
    - compilers can put out a warning about unreachable code

# Function parameters and arguments
- **pass by value**: all parameters of a function are treated as variables and value of each argument is copied into matching parameter
- argument is the **initializer** for a parameter

# Local scope
- **local variables**: function parameters and body
- lifetime for a local variable is:
    - end of set of curly braces, or at the end of a function for parameters
    - is a runtime property
- variables are destroyed in the opposite order of creation when function is exited
- **in scope** or **out of scope**
    - refers to the accessibility of a variable
    - compile time property, using an identifier that is out of scope results in a compile error
- **going out of scope** is for objects at the end of their scope in which the object was instantiated
    - variable lifetime ends at the point when it is going out of scope
- best practice: define variable as close to their first use

# Forward declaration and definitions

- Compiler compiles code sequentially so **need to define function before it is called**
    - can define the functions sequentially in the file but doesn't always work
    - use **forward declaration**:
        ```
        int add(int x, int y);
        ```
        - tells compiler existence of an identifier before defining the identifier
        - use the *function declaration/prototype* (includes the semicolon)
        - do not need to specify the names of parameter
            ```
            int add(int, int);
            ```
        - if we have the forward declaration but did not define the function:
            - if we don't call it, compiles fine (compiler just needs the **pure** declaration - does not have a definition)
            - if we do call it, linker will complain (linker needs the definition)
    - variables and user-defined types also have forward declaration
- ODR (one definition rule)
- All definitions are declarations, but not all declarations are definitions.

# Multi-file projects

- compiler can compile the files in any order but will not remember any definitions in other files
- define function in file 1 and use in file 2, file 2 must use the forward declaration
    - file 1:
    ```
    int add(int x, int y){
        return x + y;
    }
    ```
    - file 2:
    ```
    int add (int x, int y);
    int main(){add(1,2);}
    ```
    - linker will connect the functions


# Namespaces
- **naming collision/conflict**: two identical identifiers in same program (programs can contain multiple files)
    - same file: compiler error
    - separate files: linker error
- **namespace**: to keep function names from conflicting with each other
    - scope region (*namespace scope*) where we can declare names inside it
    - all names must be unique in the namespace
    - **global namespace**: any name that's not inside a class, function or namespace
        - only declaration and definition statements can appear in the global namespace
        - variable definitions are discouraged
        - assignment statements will cause a compile error
    - **std namesapce**
- to use an identifier inside a namespace, we have to tell the compiler that the identifier is in a namesace
    - explicit namespace qualifier (ex. `std::`)
        - `::` is the **scope resolution operator**
        -  `namesapce::name` (if no namesapce on the left, then the global namespace is assumed)
        - **qualified name**: identifier that includes a namespace prefix
    - directive statement (ex. `using namespace std`)
        - can use the name without using the namespace prefix
        - avoid doing thing because of current or future conflicts

# Preprocessor
- **translation**: prior to the compilation phase
    - *translation unit*: code file with translation applied
- **preprocessor**: separate program that manipulates text in code file
    - scans through code file looking for *preprocessor directives* (instructions that start with `#` and end with newline to perform text manipulation tasks)
    - does not modify original code files, all text changes are temporary in-memory or using temporary files
- **includes** (`ex. #include <iostream>`)
    - replaces `#include` directive with the preprocessed contents of the included file
    - used for header files
- **macro defines** (`#define`)
    - rule that defines how input text is converted into replacement output text
    - *object-like macros*
        - can be defined like `#define identifier` 
            - any further occurence of the identifier is removed and replaced with nothing
        - or `#define identifier substitution_text`
            - any further occurence of the identifier is replaced with the substitution_text 
            - identifier is typed in all caps with underscores for spaces
            - legacy code, should be avoided
    - *function-like macros* (act like functions)
- **conditional compilation** (`#ifdef`, `#ifndef`, `#endif`)
    - specify under what conditions something will/will not compoile
    - `#idef` checks whether an identifier has been `#define`d with the code between it and the matching `#endif` compiling if the identifier is defined and ignored if not
    - can use `#if defined(NAME)` or `#if !defined(NAME)`
    - `#if 0` to exclude a block of code
- macros don't affect other processor directives, so the directives won't get replaced
- output of the preprocessor contains no directives at all, so they are resolved and stripped out before compilation
- directives are resolved from top to bottom on a file-by-file basis with no function scope
    - lifetime is from point of definition to end of the file it is defined in

- **Header files**
    - **header file**: put all forward declaration in one file
        - `.h` or `.hpp` extension
        - contains declarations not definitions (otherwise it is a violation of the one definition rule)
        - consists of *header guard* and the contents of the header file (the forward declarations)
        - paired with code files (header files provide forward declarations for corresponding code files)
        - include it by `#include "filename.h"` in the corresponding code file and other files where its declarations are used
            - include it in the corresponding code file to catch errors at compile time in the pure declaration instead of link time
    - angled brackets vs quotes
        - gives preprocessor a clue to where header files are located
        - angle bracket: header file that was not manually written - it will search only in directories specifed in `include directories` (configured as part of compiler setting, defaults to directories containing header files that comes with compiler or OS)
        - quotes - searches header file in current directory, if cannot find a matching header then it will search `include directories`
    - **transitive includes**: the additional header files that were include in an header file, included implicitly
        - avoid by explicitly including all header files required, do not rely on transitive includes
    - if header files were written properly and everything is `#include`d, then order of inclusion doesn't matter
        - if not, then order like this: paired header file, other headers in project, 3rd party headers, standard library header
        - alphabetize each grouping
        - this will maximize changes that missing includes will be flagged by compiler so they can be fixed

- **Header/include guards**
    - for duplicate definitions when a header file gets included more than once **in a single file**
    - conditional compilation directives:
    ```cpp
    #ifndef SOME_UNIQUE_NAME
    #define SOME_UNIQUE_NAME

    // declarations, definitions

    #endif
    ```
    - convention is for the name to be the fill filename of the header file (but be wary of same filenames in different directories)
    - header should contain only forward declarations and not definitions since header guards will still include the contents for `#include` in separate files
        - header guards are not strictly necessary if we avoid definitions in header files, but it is usefuly for non-function definitions like user-defined types
    - `#pragma once`
        - same purpose as header guards, shorter and less error-prone
        - not official, use traditional header guards for max portability