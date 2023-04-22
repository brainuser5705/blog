# Operators
- **compound expression**: multiple operators
    - *precedence*: determines order in which operators are evaluated
        - highest level of precedence are evaluated first
    - *associativity*: if operators have same precedence level, then associativity tells compiler to evaluate left to right or right to left
- [table](https://www.learncpp.com/cpp-tutorial/operator-precedence-and-associativity/#:~:text=2%20%3D%206.-,Table%20of%20operators,-The%20below%20table)
- can use parentheses
    - expressions with single assignment operator don't need the right operand to be wrapped
- order of evaluation of expressions and functions are unspecified
    - precedence and associativity rules only tell how operators are evaluated
    - compiler is free to choose evaluation order for expressions/functions in compound expressions
    - best practice: ensure that expressions are not dependent on order of evaluation

# Arithmetic operators
- unary arithmetic operators: `+` (plus - value of operand) and `-` (minus- negation)
    - only take one operand
- binary arithmetic operators: `+`, `-`, `/`, `*`, `%`
    - floating point division:
        - if either operand are floating point values
        - returns floating point
    - integer division:
        - if *both* operandsd are integers
        - drops the fraction
        - to keep the fraction, use `static_cast<>` to convert integer to floating point number
    - dividing by zero causes program to crash

# Arithmetic assignment operatos
- `=`, `+=`, `-=`, `*=`, `/=`, `%=`

# Modulus (remainder operator)
- returns remainder after integer division
- only works for integer operands
- works with negative operands
    - `x % y` always return results with sign of `x`

# Exponentiation
- use `pow()` function in the `<cmath>` header (e.g. `double x{std::pow(3.0, 4.0)}` evaluates to $3^{4}$)
- parameter and return value are type `double`
- results may not be precise due to rounding errors in floating points (even with integer arguments)

# Incrementing and decrementing variables
- prefix (`++x` - pre-increment)
    - operand is incremented/decremented first, then expression is evaluated or 
- postfix (`x++` - post increment)
    - copy of operand is made, then operand (not copy) is incremented/decremented, finally copy is evaluated
    - takes more steps than prefix, so may not be as performant (prefer prefix)

# Side effect
- function or expression has **side effect** if it does anything that persists beyond life of function/expression
    - ex. changing value of object, input/output, updating gui
- could lead to unexpected results based on how compiler defines the order in which function arguments are evaluted (e.g. `add(x, ++ x)` or `x + ++x`)
- avoid by ensuring all variables that have a side effect is used only once in given statement

# Comma
- `x, y` : evaluate x, then y, returns value of y
- evaluate multiple expressions whenever single expression is allowed, returns right operand
- lowest precedence
- statement written with `,` would be better written as separate statements except in for loops
- often used a separator like in function definitions/calls which do not invoke comment operators
    - should not use to declare multiple variables (e.g `constexpr int z{ 3 }, w{ 5 };`)

# Conditional operator
- ternary `(condition) ? expression1 : expression2`
- shorthand, can help with readability
- low precedence - convention to put conditional part in parentheses for readability and to ensure precedence is correct
