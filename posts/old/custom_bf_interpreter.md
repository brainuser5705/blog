## 3/11/23

### What I did
- Create a new React app
- Plan for today: implement everything except the ide features

### Notes
- User experience
    - Replace feature
        - what it does: when you edit the symbols, the code that is in the text editor will change
        - how:
            - text replace
                - does not allow for in-sync replacement
                - the solution would be to make sure that the symbols are set before replacing
            - array of tuples (symbol, symbol type) and an algorithm that goes through the array and modifies accordingly
                - does allow for in-sync replacement
            - translation
                - translate old code to bf code, then translate new bf code to new symbols
                - does allow for in-sync replacement
- https://stackoverflow.com/questions/16330404/how-to-remove-remote-origin-from-a-git-repository

## 3/12/23

- Sync updates for button and input
    - internal symbol map reflects UI
- Colored input
    - need to fix bug: external parent valid state should be updated separately from the child (inputs) valid states
    - easiest solution would be to extract the input state variables and put it in the parent
        - but i want to avoid doing that because it'll be a lot of states to work with

## 3/20/23

- valid symbol state in app
- now all components (input and variant button) are in sync
- automatic replacement is harder than i thought, I will just focus on making the actual interpreter right now and worry about replacing latter.

## 3/21/23

- lexical tokens, source code would update state variable in App
- Source code would update the lexical tokens
- changing the symbols would update the lexical tokens which would in turn update the source code (might need to do double translation)

- we need lexical tokens because it would make highlighting for the debugger easier

- actually lexical tokens might really be overkill - I could instead just do the translation and having the highlighting be done via an offset variable

```
apple banana orange

apple -> anna

apple -> a
a banana orange

a -> an

an bannannan orannge
```

```
(apple, increment) -> (anna, increment)

(apple, increment) (banana, decrement) (orange, shift_right)

(apple, increment) -> (a, increment) # checks that is the same symbol TYPE
(a, increment) -> (an, increment)
```

```
apple banana orange

apple -> anna


apple -> a
+ - >
a banana orange

a -> an
+ - >
an banana orange
```

- app
    - state:
        - map of types and their symbols
        - array of types representing the code
    - pass map as prop to symbol inputs
    - pass array and map as props to textbox
    - on change:
        - symbol inputs will change the map
        - textbox will change the array (lexing is done at this stage)
- replace functionality
    - triggered by change to symbol input (change to the map)
    - the textbox should change automatically since the map defines the textbox


- execution:
    - pass the array of types to the parser


- textbox input
    - state:
        - array of symbol inputs
    - when executed:
        - loop through the symbol inputs and get their states

- symbol inputs
    - state
        - const type
        - value
    - on change
        - need to update the value
        - also update the textbox input?
    