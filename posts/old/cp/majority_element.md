# [Majority Element](https://leetcode.com/problems/majority-element/)

## Problem Statement

Given an array nums of size n, return the majority element.

The majority element is the element that appears more than ⌊n / 2⌋ times. You may assume that the majority element always exists in the array.

---

## Thoughts


---

## My Solution

```python
class Solution(object):
    def majorityElement(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        
        maj_count = len(nums)//2
        
        nums = sorted(nums)
        print(nums)
        
        count = 1
        i =  0 # initialization
        for i in range(len(nums)-1):
            
            if count > maj_count:
                break
            elif nums[i] == nums[i+1]:
                count += 1
            else:
                count = 1
                
        return nums[i]
```

### **Idea**

First, sort the array so that numbers that are equal are next to each other. Then, traverse the array one element at a time, keeping count of how many times it has appeared. If it appears again, then add one to it (and then check if it is larger than the majority count in the next loop). If it does not appear, reset the count to 1. Once the loop is finish, return the last element checked.

### **Complexity**

This is $O(n * log n + n)$ because you have to sort and then go through all the numbers in the worst case.

---

## Other solutions

### **Sorting**
```python
class Solution(object):
    def majorityElement(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        
        nums = sorted(nums)
        
        return nums[len(nums)//2]
```
- The majority element takes up more than half the array.
- Given the array is sorted, the majority element is always at index $\lfloor {n/2} \rfloor$. Also $\lfloor {n/2} \rfloor - 1$ if the length of the array is even (the one element outside of the half).
- Check out the graphic in the [solution](https://leetcode.com/problems/majority-element/solution/#:~:text=distinct%20elements.-,Approach%203%3A%20Sorting,-Intuition)

### **Bit manipulation**
```python
class Solution:
    def majorityElement(self, nums: List[int]) -> int:
        n = len(nums)
        majority_element = 0
        
        bit = 1
        for i in range(31):
            # Count how many numbers have the current bit set.
            bit_count = sum(bool(num & bit) for num in nums)

            # If this bit is present in more than n / 2 elements
            # then it must be set in the majority element.
            if bit_count > n // 2:
                majority_element += bit
            
            # Shift bit to the left one space. i.e. '00100' << 1 = '01000'
            bit = bit << 1
                
        # In python 1 << 31 will automatically be considered as positive value
        # so we will count how many numbers are negative to determine if
        # the majority element is negative.
        is_negative = sum(num < 0 for num in nums) > (n // 2)

        # When evaluating a 32-bit signed integer, the values of the 1st through 
        # 31st bits are added to the total while the value of the 32nd bit is 
        # subtracted from the total. This is because the 32nd bit is responsible 
        # for signifying if the number is positive or negative.
        if is_negative:
            majority_element -= bit
        
        return majority_element
```
- Look at each individual bit for the numbers, and find the majority of those bits since the majority element will be present in n // 2 bits.
- Need to look at each number and see if it is negative to determine if the majority element is negative

### **Divide and conquer**

### **Boyer-Moore Voting Algorithm**
- 1 if equal to first num in prefix, -1 if not equal to first num in prefix
- discard the prefix subarray if the count is 0 because we have discarded equal numbers of non-majority elements and majority elements
- eventually there will be a subarray where the count is not equal to 0 and the candidate num is the majority element