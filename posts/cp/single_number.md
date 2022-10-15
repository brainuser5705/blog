# [Single Number](https://leetcode.com/problems/single-number/submissions/)

## Problem Statement
Given a non-empty array of integers nums, every element appears twice except for one. Find that single one.

You must implement a solution with a linear runtime complexity and use only constant extra space.

Example 1:
```
Input: nums = [2,2,1]
Output: 1
```

Example 2:
```
Input: nums = [4,1,2,1,2]
Output: 4
```

Example 3:
```
Input: nums = [1]
Output: 1
```
---

## Thoughts

The constraints *linear time complexity* and *constant space* was throwing me off. I thought constant space would meant there was no dynamic changing of space which means any extra data structure (to store information like number of occurences) would be the solution.

Update:
Yep so it means that the space needed doesn't scale with the size of the input. So for all problems, it will remain the same amount of space.

---

## My Solution

```python
class Solution:
    def singleNumber(self, nums: List[int]) -> int:
        
        if len(nums) == 1:
            return nums[0]
        
        nums = sorted(nums)
        for i in range(len(nums)-1):
            
            if i == 0 and (nums[i+1] != nums[i]):
                return nums[i]
            elif not(nums[i] == nums[i+1] or nums[i] == nums[i-1]):
                return nums[i]
            
        return nums[i+1]
        
```

### Idea
1. Special case: if only one number in the array, return that number
2. Sort the array
3. Go through array until the last number
    - If i = 0, check if the next number is the same (since numbers should appear twice), else return the number
    - else check if either the prev number or next number is the same, else return the number
4. if the function still hasn't return a number, that means the last number of the array is the number

Note: I could have included a check for the last number in the conditional (if `i == len(nums)`), but we can leave it since the array will end up at the last number anyways.

### Complexity

Time: O(n) since we're going through the array one element at a time  

Space: O(n), `sorted()` might require n extra space

---

## Other solutions

### XOR
```python
a = 0
for i in nums:
    a ^= i
return a
```

How does `^` work?
1. `0 ^ a` or `a ^ 0` = a
2. `a ^ a` = 0
3. `(a ^ b) ^ b` = a 
    - so doing operation twice brings you back to the same number)
    - xor is associative = `a ^ (b ^ b) = a ^ 0 = a`

In this example: `[2,2,1]`  
This works because `2 ^ 2` = 0 and `0 ^ 1` would be 1.

in this example: `[4,1,2,1,2]`  
This works because xoring 1 and 2 will be done twice (thus bring it back to the same number), leaving 4 as the number.

Time Complexity  
Time - O(n)
Space - O(1)

### Invalid : hash map
1. Go through the array
2. if number is not in keys, add it or else pop it off
3. the unique number will be the only remaining key in the map

### Invalid: set
`return 2 * sum(set(nums)) - sum(nums)`  
from
`2*(a+b+c) - (a+a+b+b+c) = c`