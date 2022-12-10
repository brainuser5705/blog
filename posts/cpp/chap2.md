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
- **pass by value**: all parameters of a function are treated as variables and value of each argumetns is copied into matching parameter
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