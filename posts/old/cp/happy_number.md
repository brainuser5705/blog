# [Happy Number](https://leetcode.com/problems/happy-number/)

## Problem Statement

Write an algorithm to determine if a number n is happy.

A happy number is a number defined by the following process:

Starting with any positive integer, replace the number by the sum of the squares of its digits.
Repeat the process until the number equals 1 (where it will stay), or it loops endlessly in a cycle which does not include 1.
Those numbers for which this process ends in 1 are happy.
Return true if n is a happy number, and false if not.


---

## Thoughts


---

## My Solution

```python
class Solution(object):
    def isHappy(self, n):
        """
        :type n: int
        :rtype: bool
        """
        
        # keep a set of the past numbers to see if it cycles back
        
        s = set({n})
        
        while (n != 1):
            
            total = 0
            while n != 0:
                total += (n % 10)**2
                n = n // 10
                
            if total in s:
                break
            else:
                s.add(total)
                n = total
        
        return n == 1
```


### **Idea**

Either n will reach 1 or it will loop in a cycle. Based on that, we can keep track of all the n(s) we go through by putting them in a set. Everytime we have a new n that is not 1, we check if it's already in the set. If so, we break out of the loop otherwise we add it and continue the loop.

### **Complexity**

Depends on the number.

---

## Other solutions


### **Hashset Approach**

*Note: Why don't we handle the third case where the n goes to infinity?*
We know that it will never do that because given a certain number of digits, the largest number with that amount of digits will never be more than a certain value. For example, the largest number of 3 digits is 999. Based on the happy number, 999 would lead to the next number as 243. Numbers with 4 digits go down to 3 digits, numbers with 5 digits become 4 digits, meaning for any number of digits the cycle can never go above 243. So we would either "get stuck in a cycle below 243 or go down to 1" (leetcode solution) or in other words, "once the number is below 243, it is impossible to go above 243".

#### Time complexity

- Finding the next number is $O(log n)$ because we going through the number one digit at a time and the number of digits is $log n$ (base 10).
- For the total number in the chain (total numbers to go through until the code terminates), we know that once the number is below 243, it will not take more than another 243 steps to terminate. Each of these numbers has at most 3 digits. The time complexity for this is $O(243 * 3)$. 
- For numbers above 243, the worst cost will be $O(log n) + O(log log n) + O(log log log n)...$ which reduces to $O(log n)$. 
- So in total the time complexity is $O(243*3 + log n + log log n + log log log n + ...) = O(log n)$. The length of the chain is insignificant to the cost of calculating the next value for the first n.

#### Space complexity
- Same as the time complexity
- For optimization, we could store numbers if they are below 243.

### **Floyd's Cycle-Finding Algorithm**

- implicit linked list structure (one number leads to another number) which means the problem is the same as *finding a cycle in a linked list*
- Use two pointers: a hare (fast pointer) and a tortoise (slower pointer)
    - the hare moves two nodes at a time
    - the tortoise moves one node at a time
- If n is a happy number, the hare will reach 1 before the tortoise (`while (fastRunner != 1`)
- If n is not a happy number, the hare and tortoise will eventually end up on the same number. (`&& slowRunner != fastRunner`)

#### Time complexity

- if there is no cycle then the total cost is $O(2* log n)$ (two pointers) which reduces to *$O(log n)$.
- if there is a cycle: the hare will get one step close to the slow runner at each cycle. "If there are $k$ numbers in the cycle and they started at $k-1$ steps apart, then it will take $k-1$ steps for the fast runner to reach the slow runner." This is constant complexity.
- in total, the dominating operation is $O(log n)$.

### **Hardcoding the only cycle**

- All numbers will eventually be less than 243
- So in this case, we can find all the cycle for numbers smaller than 243
- From there we can find the only cycle from which all other numbers on the chain lead into the cycle

#### Time complexity
O(log n)