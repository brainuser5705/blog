---
layout: post
---

A "encouraging" spreadsheet to keep me track of doing what I should be doing

--- 

**TDLR: I made a spreadsheet that gives me a clearer motivation to do work than other productivity systems. Ironically I made this spreadsheet and post when I was not motivated to do my work. Or maybe it does make sense, because I haven't used the spreadsheet yet so I'm just anticipating it!**

I have not been much of a "productivity" junkie, but like any college CS kid out there, you want to make the most efficient use of your time not just in your programs but in your life. I tried a few "systems" (to-do list, plan out day, calendars) but I never really stuck to them. I find that these systems typically provide the means of *tracking* your work, but fail to keep track of your *progression*, its *relative importance*, among other things. Another key element is that the *motivation* to do you work can't entirely be faciliated in these "systems", instead it is internal and something YOU have to remind yourself of.  

So I set up a pretty crude, but simple Google Sheets that takes all of the aforementioned factors into account and most importantly, provides a **CLEAR** idea of what I should (emphasize on *should*) be doing. Unless you are a robot, mentally toughened ex-SEAL or have some crazy monk ability to not think, it's tough to stick to your plan to a T. It's not just in a macro sense, but even within the tasks. For example, you don't want to do the assignment you said you'll do because another one is easier. Or you end up daydreaming as you try to avoid thinking about work. I guess you could use the Pomodoro method but you could only do so much. The solution I see is that all you need is a bit of motivation. And that motivation is in the form of three main things: 

1. Importance (Is it mandatory or just something I want to do?)
2. Urgency (When is this thing due?)
3. Progress (How much have I finish?)

When you take these three factors and "add them up", it produces an external motivation to keep you in line.   

I think my writing (especially **my** writing) will do little to explain it, so let me break down the structure and eventually the usage of the Google sheets. I will also have a decidated section about the *"motivation equation" (ME)* (I like the rhyme). And mainly as reference to myself, I will explain the formulas I used.

---

## Motivation Equation

![](/images/sheet1.png)

As you see, there are the *Importance*, *Urgency*, and *Progress* columns that form the ME. Here is the key that I personally use. *Of course if you are adopting this system, you can use any scale you like, but would ultimately have to modify the ME.*

|Column|Key|Meaning|
|------|---|-------|
|Importance|h|Helpful (studying, making tools, teaching myself, something that would benefit me in some capacity)|
|Importance|m|Mandatory (no choice, I have to do it)|
|Urgency|n|Now (*days due* $\leq 1$, aka you're screwed if you don't finish this today)|
|Urgency|e|Edge ($2 \leq$ *days due* $\leq 4$, aka you might really want to start working on it)|
|Urgency|s|Slow (*days due* $\geq 4$, aka take my time)|
|Progress|n|Nothing (also includes "little work")|
|Progress|a|Almost done|

## Usage

1. You have an assignment (booo) you want to put into the sheet.
2. You input the basic properties (*subject*, *type*, *assignment*, *current goal*) so you know what you need to do.
3. You mark down the ME properties, as well as the *finish by* date which is used in the calculation of ME's *Urgency*.
4. *Game Plan* will determine from your ME, what your next actions are with the activity in terms of working on it.

So in other words, **Game Plan** is the result from the ME. It's the motivation that lets you know **WHAT YOU SHOULD BE DOING** rather than leaving you to stare blankly at your todo list with no clear idea of why you are even doing work.

## Formula Explained

Some basic combinatorics tells us that we have ($2$ importance features $*$ $3$ urgency features $*$ $2$ progress features) = at most $12$ possible game plans/motivators we can use.  

The reason why I say *"at most"* is because some combinations don't really matter to me. For instance, I don't really care if something that has an *Importance* value of *(h)elping* is due soon or not. So I do not need any special motivator for each *(h)elping* case, dropping my total number of game plans to $7$ (the math is a bit strange, but just know that I can reduce the number of motivation cases I need to make).

Eventually nailing down to only the essential, here are the case plans I came up with:

|Importance|Urgency|Progress|Motivation|
|----------|-------|--------|----------|
|(h)elpful|* | *| "Could benefit you" (so I might want to work on it) |
|(m)andatory|(n)ow|(n)othing|"DO IT NOW" (all caps so it likes someone is yelling at me...font pyschology?)|
|(m)andatory|(n)ow|(a)lmost done|"You should really do it today"|
|(m)andatory|(e)dge|(n)othing|"Definitely start tomorrow"|
|(m)andatory|(e)dge|(a)lmost|"Just a bit more and you're done!"|
|* |(s)low| *| "Do a bit each day and you'll be fine" |

I also have a very special "motivation" for when I complete a task i.e. I check off the "*Done*" column checkbox: "Nice job! Treat yourself with some good anime or something lol". Breaks are very important so I don't burn out so I need to remind myself to take them.  

## Calculations and Google Sheets Functions

*Importance* and *Motivation* are the two columns that I have to input manually as I don't have an easy way to automate it (it's too subjective). But I can automate *Urgency* and you can view the criteria I used in the first table above. 

Here is the Google Sheets function for it:
- `=IF(ISBLANK($F2),,DATEDIF(TODAY(),$F2,"D"))`
    This calculates the days until the assignment is due.
- `=IF(ISBLANK($G2),,IFS($G2<=1,"n",AND($G2<=4,$G2>=2),"e",$G2>4,"s"))`
    This determines the letter based on the criteria. IFS is a multiconditional statement with parameters as (expression1, value1 [,expression2, value2,...]).

To actually get the motivation, here is the procedure:
- `=IF(ISBLANK($H2),,IFS($I2="c","c",AND($H2="m",$I2="s"),"ms",$I2="s","s",TRUE,SWITCH($H2,"m",CONCATENATE($H2,$I2,$J2),"h","h")))`
    This extremely convoluted line, determines from the inputs what kind of case it should be. This is highly specific which might explain why it is so ugly.
- ```
    =IF(ISBLANK($K2),,
    (IFNA(IFS($A2, "Nice job! Treat yourself with some good anime or something lol", TRUE, SWITCH($K2,
    "h", "Could benefit you",
    "mnn", "DO IT NOW",
    "mna", "You should really do it today",
    "men", "Definitely start tomorrow",
    "mea", "Just a bit more and you're done!",
    "ms", "Do a bit each day and you'll be fine",
    "s", "Finish you work first, then you can do this"
    )
    ),"????")))
    ```
    This is a switch-case statement that determines what the values yield from the equation. If there are unsupported types, it will return "????" as I should probably look into what values might be wrong.
    ```

I also used *conditional formatting* for the specific motivators just so they pop out a bit more and look decently categorized.

# Drawbacks

Already I haven't even put this system to the test yet, and I can already use my "procrastinator" sense to spot out some flaws:

1. You only see red.  

Red means *Urgent* and *Important*, aka DO IT NOW! It might happen that this is the only thing that you would do and leave everything else for later, especially since the next *Urgent\Important* motivator is "Definitely start tomorrow". Eventually that will turn into red and it will be the same cycle of doing everything right before it is due.

A possible solution:  
It is still important that you work on your mandatory assignments. And like the adage goes, spread out your work (or something like that). So highlighted the mandatory assignments even though their *Urgency* might be slow and telling the motivation "Do a bit each day" will hopefully still get you to do more than just the last minute assignments.

2. Updates

This is not so much a drawback as it's good to consistently keep track of your assignments and update your progress, the memorization of keys and the not-so-streamline-productiveness of Google Sheets does make updating more of a hassle.

---

Update like a month later:

Yea, this system ended up being too complicated...lol