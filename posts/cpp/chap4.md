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
- **modulo wrapping**: divided by one grater than the largest number of the type and the remainder is kept (ex. 280 -> 280/256 with remainder 24 -> 24. 256 is wrapped around to 0)
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
- 