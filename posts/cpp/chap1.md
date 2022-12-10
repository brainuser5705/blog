# Statements
- different types of statements
    - declaration
    - jump
    - expression
    - compound
    - selection/conditional
    - iteration/loop
    - try

# Functions
- programs terminate after last statement in `main`

- syntax error is a compile-time error

# Comments
- At the library, program, or function level, use comments to describe what.
- Inside the library, program, or function, use comments to describe how.
- At the statement level, use comments to describe why.

# Variables
- All computers have **RAM** memory: like series of numbered mailboxes that can be used to hold a piece of data (**value**) while program is running.
    - not direct access but access through an **object**: region of storage that has value and other properties associated with it
    - object can be named (**variable**, name of variable is identifier) or unnamed
    - at runtime, variable with be **instantiated** : created and assigned a memory address, and be called an **instance**
    - variables must be instantiated before they can store value
- data type tells program how to interpret value in memory
    - data type of variable must be known at compile-time (cannot be changed without recompiling)

    ```cpp
    int a;
    int b;

    int a, b; // must be the same type

    int a, int b; // wrong (compiler error)
    ```

- **copy assignment**: copies value on RHS to variable on LHS
    - `width = 5;`
- **initialization**: defines/creates a variable and provide an initial value at the same time (versus assignment that gives variable a value after variable has been created)
    - value used to initialize a variable is called an *initializer*
    - `int a;` - default initialization: no initializer and leaves variable with indeterminate value
    - `int a = 5;` - copy initialization (not as common in modern cpp)
    - `int c(6);` - direct initialization: allow more efficient initialization of complex objects (not as common)
    - `int d{7};` - brace/uniform/list initialization
        - types:
            - `int d {7}` - direct brace initialization
            - `int d = { 7 }` - copy brace initialization
            - `int d {}` - value initialization
        - provide more consistent initialization syntax, can also initialize objects with list of values
        - disallows *"narrowing conversions"*: cannot brace initialize a value that a variable cannot safely hold, compiler will throw an error with `int width { 4.5 }`
            - copy or direct initialization would drop the fractional part and the compiler will only throw a warning
            - note this is only during the compilation process,; if `std::cin` receives a fractional number for an integer variable, it will truncate it
    - **value initialization**: initializes the variable to zero (**zero initialization**) or empty (appropriate to the data type)
        - use explicit initialization if value will be used
        - use value initialization if value is temporary and will be replaced

        ```cpp
        int a = 5, b =6;
        int c(7), d(8); 
        int e{9}, f{10};

        int a, b = 5; // wrong (a is uninitialized and program will have erratic behaviour)
        ```

- unused initialized variables might cause compiler to generate warnings
- can use `[[maube_unused]]` attribute to tell compiler
    ```cpp
    int main()
    {
        [[maybe_unused]] int x {5};
        return 0;
    }
    ```

# iostream
- need to include `#include <iostream>`
- contains predefined variables
    - `std::cout`: send data to console to be printed as text
        - `std::cout << "Hello, world!"` where `<<` is the *insertion operator*
        - `<<` can be used multiple times to concatenate pieces of output (`std::cout << "Hello" << "world"`)
        - `cout` stands for character output
    - `std:endl`: moves cursor to the next line by printing a newline character
        - `std::cout << "Hellow, world!" << std::endl`
        - does two jobs: move the cursor and flushes the output (any cached output shows up on the screen immediately)
        - use `\n` instead when flushing isn't necessary (`std::cout << "x is equal to " << x << '\n'`, or can embed in the text)
    - `std::cin`: reads input from keyboard using *extraction operator* `>>`
        - input must be stored in a variable 
        ```cpp
        int x{};
        std::cin >> x;
        ```
        - can input more than one variable at a time
        ```cpp
        int x{}, y{};
        std::cin >> x >> y;
        ```
        - user needs to press enter key to have input accepted

# Unintialized Variables
- default value for variable is whatever is already in the memory address
    - Initialization = The object is given a known value at the point of definition.
    - Assignment = The object is given a known value beyond the point of definition.
    - Uninitialized = The object has not been given a known value yet.
- **always initialize variables**
    - compilers might throw a warning or an error
    
# Undefined behavior (UB)
- result of executing code whose behavior is not well defined by the C++ language (ex. uninitialized variable)
- never know what the outcome will be

# Keywords and naming identifiers
- reserved words (keywords)
- special identifiers that have specific meaning when used in certain context but not reserved
- a good rule of thumb is to make the length of an identifier proportional to how widely it is used

# Whitespace
- C++ compiler ignores whitespace, is a whitespace-independent language
- newlines not allowed in quoted text
    - but quoted text separated by whitespace will be concatenated
        ```cpp
        std::cout << "Hello"
            "world!";
        ```

# Literals and operators
- **literal constant**: fixed value that has been inserted directly into source code
- **operators**:
    - number of operands an operator takes an input is called the operator's *arirty*
    - unary, bianry, ternary, nullary (zero operand, `throw operator`)

# Expressions
- **expression**: combination of literals, variables, operators, function calls that *evaluates* single value (*result*)
    - each term in expression is evaluated until single value remains
    - do not include semicolon and cannot be compiled by themselves
    - must exists as part of a statement
    - ex. `x = 5`
- **expression statements**: statement that consists of an expression followed by semicolon
    - result of expression statement is discarded before next statement is executed (**discarded-value expression**)
    - useless expression statements may produce warnings from the compiler
