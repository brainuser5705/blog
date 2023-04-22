# Tree-walk interpreter

## Scanning
- scanner takes raw source code and groups it into **tokens**
    - basically a switch statement
    - group them together into smallest sequences that still represent something
    - **lexeme**: blobs of characters, only raw substring that don't mean anything on its own
    - **token**: bundle lexeme with useful information
        - *token type*: keywords, implemented with an `TokenType` enum
        - *literal value*: convert the textual representation of the value to a living runtime object
        - *location information*: line token appears on
            - can also do column, length by keeping track of offset and length of lexeme
            - calculate the line on-demand by counting the preceding newlines
    - core is a loop
        - figures out what lexeme character belongs to, consumes it
        - when reached the end of the lexeme, it emits the token
        - loops back and continue until it reaches end of input
    - **lexical grammar**: rules that determine how particular language groups character into lexeme
        - language would be classified as a **regular language**
        - can recognize with regexes using tools like *Lex* or *Flex* that give you a complete scanner back
    - keep scanning even if error is encountered
    - to handle longer lexemes, shunt over to lexeme specific code that keeps consuming/advancing the characters until end of line is reached
    - `peek()` does **lookahead**
        - does not consume the character, *one character of lookahead*
    - `advance()` and `peek()` are fundamental operators and `match()` combines them
    - **maximal munch**: when two grammar rules can match a lexeme, whichever one matches the most characters win
        - cannot detect reserved word until we reached end of identifier
- separate code that generates error from code that reports them