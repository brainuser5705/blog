# The Regular Expression Leetcode Problem

**Try it out for yourself**: [https://leetcode.com/problems/regular-expression-matching](https://leetcode.com/problems/regular-expression-matching)

## My Attempt

```python
class Solution(object):
    
    def __init__(self):
        self.d = {}
    
    def isMatch(self, s, p):
        """
        :type s: str
        :type p: str
        :rtype: bool
        """
        return self.checkMatch(s,p,None)
        
        
    def checkMatch(self, s, p, prev, star=False):
        
        if s == '':
            if p == '':
                # String and pattern is completely matched
                return True
            else:
                if len(p) != 1 and p[1] == '*':
                    # skipping if character doesn't match and next char is *
                    return self.checkMatch(s,p[2:],prev)
                elif prev in self.d and p[0] == prev:
                    # extra character at end
                    return self.checkMatch(s,p[1:],prev)
        elif s != '':
            if p == '':
                if (s[0] == prev or prev == '.') and star:
                    # Continuing off a *
                    return self.checkMatch(s[1:],p,prev, star=True)
                else:
                    # String is not fully matched
                    return False
            else:
                if s[0] == p[0] or p[0] == '.':
                    # Matching chars or pattern is any char
                    # See if it's continuing off of * 
                    return self.checkMatch(s[1:],p[1:],p[0], star=star)
                elif p[0] == '*':
                    # Move to next character in pattern
                    return self.checkMatch(s,p[1:],prev, star=True)
                    self.d[prev] = True
                elif s[0] != p[0]:
                    # Not equal characters
                    if (s[0] == prev or prev == '.') and star :
                        # Continuing off a *
                        return self.checkMatch(s[1:],p,prev, star=True)
                    elif len(p) != 1 and p[1] == '*':
                        # skipping if character doesn't match and next char is *
                        return self.checkMatch(s,p[2:],prev, star=True)
                    else:
                        # No match
                        return False
        else:
            return False
```

If you didn't read all of that, that's good. Cause it's bad. Very bad. Super duper bad. Oh so very bad.

You can see I used recursion, it's actually one of the strategies that Leetcode tagged this problem as. My approach was matching characters step by step, and using string slicing to continue that iterative process throughout the whole string. In each call, the function would see if it matches any of the hardcoded rules (the if-elif-else statements) and call recursively with the appropiate string slicing along with some other parameters. And yea that didn't work. Apparently Leetcode allows you to look at failed cases, so I was hardcoding the rules literally after every submission.

Not wanting to be stuck on this problem for the rest of my life, I finally rest my ego and looked at the solutions. And omg, what is wrong with me. How is it that simple.

Check it out here: [https://leetcode.com/problems/regular-expression-matching/solution/](https://leetcode.com/problems/regular-expression-matching/solution/)