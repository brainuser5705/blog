# [Leetcode - 8. String to Integer (atoi)](https://leetcode.com/problems/string-to-integer-atoi/description/)

# My Solution

## Solution 1
```python
class Solution(object):
    def myAtoi(self, s):
        """
        :type s: str
        :rtype: int
        """
        
        s = s.lstrip(' ')
        
        sign = '+'
        if len(s) > 1 and s[0] in '-+':
            sign = s[0]
            s = s[1:]
        
        num = ''
        for char in s:
            if char.isdigit():
                num += char
            else:
                 break
                
        if num == '':
            num = 0
        else:
            num = int(sign + num)
            
        if num < -(2**31):
            num = -(2**31)
        elif num > (2**31)-1:
            num = (2**31)-1
            
        return num
```

The problem is pretty straightforward, so the solution more or less does the conversion sequentially.

## Solution 2
```python
class Solution(object):
    def myAtoi(self, s):
        """
        :type s: str
        :rtype: int
        """

        s = s.strip()

        if len(s) == 0:
            return 0

        sign = 1
        if s[0] == '-':
            sign = -1

        i = 0
        if s[0] == '-' or s[0] == '+':
            i = 1

        num = 0
        while i < len(s) and s[i].isnumeric():
            num *= 10
            num += int(s[i])
            i += 1

        return min(num * sign, 2**31-1) if sign == 1 else max(num *sign, -2**31)
```

This solution is more deconstructed. Instead of gathering all the characters and then converting to an int altogether, it converts each digit one by one.

Also instead of string slicing in the *Solution 1*, it uses a manually set index variable `i` which does required a redundant conditional.

## Solution 3

```python
class Solution:
    def myAtoi(self, s: str) -> int:

        s = s.lstrip()

        if s == '':
            return 0

        sign = '+'
        i = 0
        if s[0] in '-+':
            sign = s[0]
            i = 1

        f = i # set to i if the first value encountered is not a digit
        while f < len(s) and s[f].isdigit():
            f += 1

        if f == i:
            return 0
        
        num = int(s[i:f])
        if sign == '-': 
	        # apparently doing (sign + s[i:f]) takes up more memory but will run faster
            num *= -1

        if num < -(2**31):
            num = -(2**31)
        elif num > 2**31 - 1:
            num = 2**31 - 1

        return num
```

This approach uses finds the first and last index of the number to convert and does string slicing for the final conversion. 