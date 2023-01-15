#greedy #two-pointer
# [Leetcode - 11. Container With Most Water](https://leetcode.com/problems/container-with-most-water/)

# My Solution

```python
class Solution(object):
    def maxArea(self, height):
        """
        :type height: List[int]
        :rtype: int
        """

        max_area = 0
        for i in range(len(height)):
            for j in range(i):

                h = min(height[i], height[j])
                max_area = max(max_area, h * (i-j))

        return max_area
```

This is an $O(n^{2})$ solution. For every new line, go through the previous lines and calculate the area, keep track of the max area to return at the end.

```python
class Solution(object):
    def maxArea(self, height):
        """
        :type height: List[int]
        :rtype: int
        """

        i = 0
        j = len(height)-1

        a = 0

        while i != j:

            i_height = height[i]
            j_height = height[j]

            a = max(a, min(i_height, j_height) * (j-i))

            if i_height < j_height:
                i += 1
            else:
                j -= 1

        return a
```

This is the *greedy* solution, pretty close to the [official solution](https://leetcode.com/problems/container-with-most-water/solutions/127443/container-with-most-water/). It starts with two pointers at the first and last line. From there, it calculates the area, move the pointer with the smallest height (reducing the width), and updates the area until the pointers eventually meet. The proof that this obtains the optimal solution is [here](https://leetcode.com/problems/container-with-most-water/solutions/6089/Anyone-who-has-a-O(N)-algorithm/comments/7268/).