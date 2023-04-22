# Bits, bytes, memory addressing
- **bit**: smallest unit of memory
- **memory addresses**: memory organized in sequential units
    - each memory address stores a **byte** (group of 8 bits that are operated on as a unit)
- **data type**: tell compiler how to interpret memory
    - when value is given to an object, compiler and CPU encode it into bits to store into memory
    - when object is evaluated, bits are reconstructed into the original value

# Fundamental data types (*basic*, *primitive*, *built-in*)
| Types | Category | Meaning|
| - | - | - |
| `float`, `double`, `long double` | floating point | number with fractional part |
| `bool` | integral (boolean) | true or false |
| `char`, `wchar_t`, `char8_t`, `char16_t`, `char32_t` | integral (character) | single character of text |
| `short`, `int`, `long`, `long long` | integral (integer) | positive and negative whole numbers |
| `std::nullptr_t` | null pointer | null pointer |
| `void` | void | no type |

- integer is a specific data type, integral means "like an integer" (stored as integers in memory but behaviors vary)
-  `string` type is not a fundamental data type, but compound
- `_t` suffix is used to mean "type"

# Void
- variables cannot be defined as a void type (i.e. `void value` will not work)
- used in:
    - functions that don't return a value
    - functions that don't take parameters (`int getValue(void)`) (depreacated)

# Object sizes and `sizeof` operator
- single object can take up multiple memory addresses, amount of memory depends on the data type
- more memory an object uses, the more information it can hold
    - object with *n* bits can hold $2^{n}$ unique values
- size of given data type is dependent on the compiler/computer architecture
- fundamental type guaranteed minimum sizes:

| category | type | min size |
| --- | - | - |
| boolean | `bool` | 1 byte |
| character | `char` | 1 byte (always) |
| - | `whcar_t` | 2 |
| - | `char16_t` | 2 |
| - | `char32_t` | 4 |
| integer | `short` | 2 |
| - | `int` | 2 (could be 4 depending on machine) |
| - | `long` | 4 |
| - | `long long` | 8 |
| floating point | `float` | 4 |
| - | `double` | 8 |
| - | `long double` | 8 |

- `sizeof(type/variable)`: determine size of data type on particular machine
    - cannot be used on the void type since it has no size
    - note that the variable does not have to be assigned a known value, by instantiating it, the memory address is assigned
- less memory does not necessarily mean faster programs, some CPU optimize for a particular size (ex. 32 bit)

# Signed integers
- larger integers can hold bigger numbers
- number's sign is stored in a single bit (**sign bit**) so signed integers can hold both positive and negative numbers
- omit additional `int` keyword at the end (ex. prefer `long long` instead of `long long int`)
- can be declared with optional `signed` keyword (ex. `signed short`, `signed long long`)
    - not used because of redundancy since integers are signed by default

# Signed integer ranges
- **range**: set of specific values a data type can hold
    - determined by its size and whether it is signed
- n-bit signed variable has range of $-2^{n-1}$ to $2^{n-1}-1$ (ex. 8 bit has range of -128 to 127 ($2^{7}$))

# Integer overflow
- occurs when we try to store value that is outside the range of a type
- data is lost and results in undefined behavior

# Integer division
- division with two integers always produces an integer result (fractional part is truncated)

# Unsigned integers (avoid using)
- integers that hold non-negative numbers (ex. 8 bit has range from 0 to 255, in general 0 to $2^{n}-1$)
- use with `unsigned` keyword

# Unsigned integer overflow
- **modulo wrapping**: divided by one greater than the largest number of the type and the remainder is kept (ex. 280 -> 280/256 with remainder 24 -> 24. 256 is wrapped around to 0)
- can wrap around the other direction with negative numbers (-1 -> 65535 for unsigned 2 byte int)

# When to avoid and when to use unsigned numbers
- avoid:
    - can produce unexpected results since it cannot store negative numbers and will wrap around
    - when mixing signed and unsigned integer, the signed integer will be converted to unsigned
- okay to use:
    - dealing with bit manipulation
    - when well-defined wrap around behavior is required
    - array indexing

# Fixed-width integers and `size_t`
- no fixed size because of originally C let implementers choose size for data types to best perform for target architecture
- now we can use **fixed-width integers** (in `stdint.h` header, `#include <cstdint>` is defined inside the `std` namespace) that will be the same size on any architecture

| Name | size |
| - | - |
| `std::int8_t ` | 1 byte signed (treated like a signed char - system dependent so avoid this type) |
| `std::uint8_t` | 1 byte unsigned (treated like unsigned char) |
| `std::int16_t`, `std::uint16_t` | 2 byte signed/unsigned (use this instead of 8 bit types) |
| `int32_t` | ... |
| `int64_t` | ... |

- downsides:
    - not defined on all architectures, only exist where there are fundamental types matching their width and follow certain binary representation
    - may be slower than a wider type on some architecture (using `int32_t` when your CPU is  faster at processing 64 bit integers)

# Fast and least integers
- most slow programs are caused by *memory usage* rather than CPU performance bu it is hard to know without measuring
- two alternative sets of integers
    - fast types (`std::int_fast#_t`, `std::uint_fast#_t`): fastest type with width of at least # bits (can be more than # bits, ex. `int_fast16_t` -> 32 bits since machine can processs 32 bits faster)
    - least types (`std::int_least#_t`, `std::uint_least#_t`): smallest type with width of at least # bits
- size of fast and least integers can vary for different architectures

# Best practice
- Prefer: 
    - Prefer int if the number fits, don't worry about size
    - Prefer `std::int#_t` when storing value that needs guaranteed size
    - Prefer `std::uint#_t` when doing bit manipulation or well-defined wrap-around behavior is required
- Avoid: 
    - unsigned types for holding values
    - Avoid using fast/least types in favor of fixed-width types
    - 8 bit fixed-width integer
    - compiler-specified fized-width integers

# `std::size_t`
- the type that `sizeof()` returns
- defined as an unsigned integral type and used to represent size or length of objects
- `std::size_t` varies in sizes depending on system (can do `sizeof(std::size_t`) to find size)
    - guaranteed to be at least 16 bits, but most of time is equivalent to address-width of application (ex. on 32 bit application -> will be 32 bit unsigned integer)
    - large enough to hold size of larglest object creatable on system in bytes

# Scientific notation
- significand * $10^{exponent}$
    - significand writtend with one digit before the decimal point
    - trim off leading zeros and trailing zeros (if original number has no decimal point, eg. 600.410 would be 6.00410 - not trimed because orig num had decimal point)
    - trailing zeros in whol number with no decimal points are not significant
    - *significant digits*: number of digits in significand (part before e). more digits means more *precision* of the number
-in Cpp, use `e` to represent "multiply by 10 to power of" for large numbers
    - precision of number does not matter in Cpp (87 is treated the same as 87.00)

# Floating point numbers
- floating point means that decimal point can "float" (support variable number of digits before and after decimal point)
- does not define actual type but min sizes, modern architecture almost always follow IEEE 754 binary standard (float - 4 bytes, double - 8, long double - 8, 80 bites - 12, 16 bytes)
| type | size | typical size |
| - | - | - |
| `float` | 4 bytes | 4 |
| `double` | 8 | 8 |
| `long double` | 8 | 8,12,16 |
- always include at least one decimal place (even if 0) when using floating point literals to help compiler recognize it as a floating point number
    - make sure to match with the correct type otherwise unnecessary conversion with possible loss of precision
    - don't use integer literals where floating point literals should be used
```cpp
double y{5.0}; // no f suffix means double type by default
float x{5.0f}; // f means float type
```

# Printing floating point numbers
- if decimal is 0, then `std::cout` will not print the fractional part of the floating point number

# Floating point Precision
- limited memory means floating point numbers can only store certain number of significant digits
- **precision** is affected by how many significant digits can be represented without information loss
    - `std::cout` has precision of 6 (assumes that all floating point numbers are only significant by 6 digits, minimum precision of a float) and truncate anything after
    - `std::cout` might switch to scientific notation, exponent might be padded with extra 0's depending on compiler
- number of digits of precision depends on size and particular value of floating point number

| type | precision |
| - | - |
| `float` | 6-9 digits of precision (most have at least 7) |
| `double` | 15-18 (most have at least 16) |
| `long double` | minimum of 15, 18, or 33 depending on how many bytes it occupies |

- can override default precision with an **output manipulator** function (affects how data is output) named `std::setprecision()` defined in `iomanip` header
    - ex. `std::setprecision(16)` shows 16 digits of precision
    - precision is still affected by the size of the data type and can affect non-fractional numbers as well (e.g. number has too many significant digits)
- best practice is to favor double over float unless space is at a premium, lack of precision in float can cause inaccuracies
- **rounding error**: when number can't be stored precisely and precision is lost
    - leads to outputted number being either slightly smaller or slightly larger depending on where truncation happens (value is precise to the memory limit but is not exactly the assigned value)
    - comparing floating point numbers is generally problematic
    - mathematical operations make rounding errors grow
    - never assumer floating point numbers to be exact

# NaN and Inf
- two special categories of floating point number
    - **Inf**: infinity, can be positive and negative
    - **NaN**: not a number
    - available if compiler uses IEEE 754 format for floating point numbers
- `5.0 / 0.0` is `1.#INF` (positive infinity), `-5.0 / 0.0` is `-1.#INF` (negative infinity), `0.0 / 0.0` is `1.$IND` (not a number, indeterminate)
- printing of Inf and NaN are platform specific
- avoid division by zero altogether

# Boolean values
- keyword `bool`, assign with values `true` or `false` (default initialization is false, e.g. `bool b3 {};`)
- logical NOT operator `!`
- stored and evaluated as integers 0 and 1, considered an integral type
- `std::cout` prints out 0 or 1, use `std::boolapha` to print out true and false (`std::cout << std::boolalpha` will turn on the feature, `std::noboolapha` to turn it back off)
- cannot initialize boolean with integer using brace/uniform intialization (e.g. `bool b { 4 }`) because of narrowing conversion
    - instead use copy initialization (e.g. `bool b1 = 4`) for implicit conversion from int to bool (0 for false, any other number for true)
- `std::cin` accepts 0 or 1 for boolean variables (not true or false string literals), silently failed inputs will zero-out variable to make it false
    - to allow true and false to be accepted, use `std::cin >> std::boolapha` (but 0 and 1 will not be treated as booleans)

# If Statements
- if statements only conditionally execute a single statement:
```cpp
if (condition)
    statement;
```
- `if`-`else if`-`else`
- non-boolean conditionals: the conditional expression is converted to a boolean value (non-zero is true, zero values is false)

# Chars
- `char` holds one character and is stored as an integer (**ASCII code, code point**)
    - codes 0-31 are unprintable, for formatting and control printers
    - codes 32-127 are printable chars
- single quotes (can use double quotes but that is not optimized)
- can initialize with character (e.g. `char c{'a'}`) or integer value (e.g. `char c{97}`, not preferred)
- printing chars:
    - `std::cout` outputs the character as an ASCII character
    - can be outputted directly (e.g. `cout << 'c'`)
- inputting chars:
    - `std::cin` allows for multiple characters to be inputted but the `char` variable can only hold one character at a time
    - the rest of the user input is left in the input buffer and can be accessed with `std::cin` again
- char can be signed or unsigned since both types can hold onto values between 0 and 127
- if using char to hold small integers (should never do unless optimizing for space), always specify whether it is signed or unsigned
- **escape sequences**: '\' followed by character or number, [table](https://www.learncpp.com/cpp-tutorial/chars/#:~:text=Meaning-,Alert,-%5Ca)
- avoid using **multicharacter literals** (e.g. `'56'`) since they have implementation-defined value (varies on the ompiler), not part of the C++ standard
    - make sure escape sequences use backslash otherwise `/n` will be a multicharacter literal
- `wchar_t` should be avoided unless interfacing with Windows API
    - size is implementation defined, not reliable, deprecated (still supported but replaced with something better or no longer safe)
- `char16_t` and `char32_T`
    - used to support Unicode encoding (UTF-32 or UTF-36) since Unicode code point might need 16-32 bits to represent character
    - don't need to use unless program should be Unicode compatible

# Type conversion and static_cast
- **type conversion**: converting value from type to another
    - C++ allows conversion from fundamental type to another fundamental type
    - **implicit type conversion**: when compiler does type conversion without needing to be coded in
    - not actually converted, type conversion produces a new value of the target type
    - compiler might put out a warning for possible loss of data when converting larger data type to smaller data type
        - this is why brace initializtion is the preferred initialization form (narrow conversion will throw an error)
    - **explicit type conversion**: explicitly tells compoiler to convert and ignore any loss of data
        - `static_cast` operator (`static_cast<new_type>(expression)`)
        - note that it converts expressions, variables will not be reassigned - only the value of the variable is used
        - convert unsigned to signed number (no range checking)
        ```cpp
        unsigned int u { 5u }; // 5u means the number 5 as an unsigned int
        int s { static_cast<int>(u) }; // return value of variable u as an int
        ```
        - `std::int8_t` and `std::int16_t` need to be explicitly cast since most compilers treat them as `char`s

# Const variables and symbolic constants
-  `const` keyword before or after variable type (`const int` (more common) or `int const` (called the east const style))
- const variable must be initialized during definition (eg. `const double a; a =5.0` will cause an error) since value cannot be changed via assignment
    - const variables can be initialized from non-constant variables
- camel case to name const variables
- function parameters can also be const (`void function(const int x)`)
    - value of argument is used as the intializer
    - ensures parameter value is not changed inside the function (however don't use const when passing by *value* since the value will be copied anyways)
- function return values can be const (`const int getValue()`)
    - returning const value can impede compiler optimization
    - don't use when returning by value since the value is a copy
- **symbolic constant**: name given to constant value
    - constant variables have name/identifier and constant value
    - object-like macros (`#define identifier substitution_text`)
    - prefer constant variables over object-like macros:
        - macros cannot be debug
        - macros can have naming conflicts with normal code (e.g. same name for variable and macro)
        - don't follow normal scoping rules
    
# Compile time constants, constant expressions, constexpr
- **constant expression**: expressions that can be evaluated at compile-time
- all values in the expression must be known at compiler-time, all operators and functions called must support compile-time evaluation
- ex. `int x {3 + 4}` - compiler may replace the const expression with `7`, `std::cout` is not a compile-time supported function since outputs are runtime
- makes compilation time longer, but expressions only need to be evaluated once which makes the executables faster and use less memory
- compiler only required to evaluate const expressions in contexts where value is actually required at compile time (`int x {3 + 4`} is not a constant variable and does not need to be known at compile time, so the expression is not required to be evaluated but most compilers do anyways)
- **compile-time constant**: constant whose value is known at compile time
- **compile-time const**: `const` variable is compile-time constant is intializer is constant expression
- **runtime const**: `const` variable's intiializer is not known until runtime (e.g. when initializing with a non-constexpr returning function)

# `constexpr` keyword
- there are cases where C++ requires a compile-time constant and typically we want to use compile-time constants for better optimization
- hard to know if `const` variable is compile-time or run-time const so we can use `constexpr` instead - the initializer can only be a compile-time constant otherwise the compiler will throw an error
- best practice:
    - `constexpr`: any variables whose initializer is known at compile time
    - `const`: any variables whose intializer is not known at compile time

# Constant folding for constant subexpressions
- optimization process is called "constant folding" since constant subexpressions can be optimized at compile-time even if they are used in non-const expressions

# Literals
- **literal/literal constant**: unnamed values inserted directly into code that cannot be reassigned
- type of literal is deduced from value
    - to change the default type of a literal, a suffix can added to the end of the value [table](https://www.learncpp.com/cpp-tutorial/literals/#:~:text=const%20char%5B14%5D-,Literal%20suffixes,-If%20the%20default)
    - prefer uppercase suffixes
    - cases to look out for:
        - generally suffixes are not needed (except for unsigned integer literals)
        - floating point numbers by default have type `double`, to make it into a float, use the suffix `f`/`F`
        - floating point literals can also be declare in scientific notation (e.g. 6.02e23 is a double literal)
        - avoid magic numbers by using `constexpr variables`

# Numeral systems
- 4 numeral systems in C++ (decimal, binary, hexadecimal, octal)
- octal literals: prefix literal with `0` (e.g. `int x{012}`)
    - octal is rarely used and should be avoided
- hexadecimal literals: prefix with `0x` (e.g. `int x {0xF}`)
    - single hexadecimal digit encompasses 4 bits since it has 16 values
    - used to represent memory addresses/raw data
- binary literals:
    - can use hexadecimal literals (e.g `0x0002` to represent `0000 0000 0000 0010` in binary)
    - we can use binary literals with `0b` prefix (e.g. `0b1010` to represent `0000 0000 0000 1010`)
- C++ allows use of quotation mark (') as separator for long numbers (e.g. `2'123` for 2123)
    - purely visual
- `std::dec`, `std::oct`, `std::hex` I/O manipulator
```cpp
std::cout << std::hex << x; // set output to be hexadecimal
std::cout << x; // now hexadecimal
```
- `std::bitset` in the `<bitset>` header for outputting in binary
    - tell `std::bitset` how many bits to store and must be compile-time constant value
    - can be initialized with unsigned integral value of any format
    ```cpp
    #include <bitset>
    // more code
    std::biset<8> bin1{0b1111'1001};
    std::cout << std::bitset<4>{0b1010}; // temporary std::bitset

# `std::string`
- not a fundamental type in C++, instead use double quote "C-style strings"
- `std::string` and `std::string_view` string types in the `<string>` header
    - create objects (e.g. `std::string name {"John"};`)
    - can create empty strings with no value (i.e `std::string empty{}`)
- C++ does not auto convert strings to integral types
- string output works with `std::cout`
- string input:
    - `std::cin` - with operator `>>` extract string and return characters up to the first whitespace (not including), other characters are left in queue
    - `std::getline()` reads the full line of input to a string
        - two arguments: `std::cin` and the string variable
        - ex. `std::getline(std::cin >> std::ws, name)`
        - `std::ws`: **input manipulator** that tells `std::cin` to ignore leading whitespace before extraction (skips the newline from before)
- string length with member function `str_variable.length()`
    - returns unsigned integral value (`size_t`)
    - use `static_cast<int>` when assigned to an `int` to avoid compiler warnings about unsigned/signed
    - use `std::ssize(value)` get length as signed integer
- avoid passing `std::string` by value since copies of strings are expensive and should be avoided
- by default, string literals are C-style stirngs - to create string literal with type `std::string`, use `s` suffix after double-quoted string literal (e.g. `"goo"s`)
    - suffix lives in namespace `std::literals::string_literals` and can be accessed with `using namespace std::literals`
    - can use C-style string literals instead of `std::string` literals, but there are cases where they make things easier
- `constexpr std::string` is not supported and causes a compile-time error

# `std::string_view`
- when initializing/copy a `std::string`, a C-style string literal is copied into the memory allocated
    - slow process and can be inefficient if string is only used one time
- located in `<string_view>` header
- provides read-only access to existing string (C-style string, `std::string`, char array) without needing to copy the string
    - use when you need a read-only string
- full support for `constexpr`
- can be created using `std::string` intializer since it will implicitly convert from `std::string` to `std::string_view`
- does not allow implicit conversion from `std::string_view` to `std::string`
    - we can explicitly create `std::string` with `std::string_view` initializer
    - but will need to use static_cast if passing by value
- literals with type `std::string_view` are with suffix `sv`
- avoid returning `std::string_view` from function