# Fib in Brainf**k

*[If you don't know what Brainf**k is, click here!](https://esolangs.org/wiki/Brainf**k)* - The main takeaway is that *Brainf**k* is a *Turing tarpit*, essentially a programming language that achieves *Turing completeness* (or the ability to create virtually any program) with as few operations as possible. In *Brainf**k*'s case: only 8 operations.

---

Here is a internal dialogue that I had with myself during my attempt to make a program that would output the Fibonacci sequence. Obviously, it's hard to clearly describe my thought process, but basically it's how I experience the true meaning of Turing tarpit. (*You can use this [online interpreter](https://minond.xyz/Brainf**k/) to run the code if you want)

> *So the operations are pretty much similar to a Turing machine with how it shifts and increments/decrements values. I had some experience taking CS Theory with making Turing machine diagrams, so I think I can do this!*

My first idea.

> *Ok here's my thinking. Since the Fib sequence relies on two numbers and adding each one to another. I can use the `[` and `]` operations (which stimulates a while loop) to do that! All I need is at least two copies of the numbers, so that one can be used to output the sequence and the other for the while loop pointer.*


```Brainfuck
>>+>+               setup

[                   generating loop
    [-<<+<+>>>]     loop 1
    <<<             switch side
    [->>+>+<<<]     loop 2
    >>>             switch side
]
```

Here's how it would run, where each number represents a cell. The middle two numbers is where the sequence will be generated, the outer two numbers are the loop pointers.
```
0000 - before running
0011 - setup

generating begins

1110 - after loop 1
0111 - after loop 2

another generating loop begins

1211 - after loop 1
1221 - after loop 2 (hmm the fib numbers is supposed to be 2 and 3...)
```

> *So that didn't work...Oh it's because I am not making the while loop pointer resort back to the original value, so it's always going to be 1! This is harder than I thought...*

Second idea.

> *Ok, so I can't figure out a way for the loop pointer to go back to the original value, since the while loop condition is always when the pointer is equal to 0. Here's another way:*

```Brainfuck
>+>+            setup
<<              go back to cell 0
.>.             output the first two fib numbers

[               generating loop
    [->+>+<<]   shifting loop
	>.          output the next fib number
]
```

Run:
```
011 - after setup
output the 0 and 1 cell

generating begins

0021 - shifting loop
output the 2 cell

generating begins

00032 - after shifting loop
output the 3 cell

generating begins

000053 - after shifting loop
output the 5 cell

...
```

> *This only uses two cells where each cell will hold one of the two fib numbers at the current loop. Alright so now I have a way to generate the fib numbers, I just need to adjust the output so that it will print out the digits since Brainf**k's output uses ASCII values.*

A few minutes later...

> *Wait this is actually hard. What do I do if there is more than one digit in the number. How do I print out those digits separately? And the cells can only hold up to 255, so that means this algorithm only works up to that number! I know it's possible, how did other people figure this out???*

---

It was at this moment I realize that some people are insane (in a good way).

Here is a [solution](http://Brainf**k.org/fib.b) by Daniel B Cristofani.

That lead me to discover a [whole site of example BF code](https://sange.fi/esoteric/Brainf**k/). I gave up on making a Fib code, but how the f did someone make a [Mandelbrot Set generator](https://www.youtube.com/watch?v=ABnBd0VZmPI)?!!?!?

![](https://i.imgur.com/IH4fZyx.gif)




