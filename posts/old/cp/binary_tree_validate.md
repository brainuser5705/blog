# []()

## Problem Statement
Given the root of a binary tree, determine if it is a valid binary search tree (BST).

---

## Thoughts
My first approach was too "localized" meaning I was only checking relative to the current root. But the tree needs all nodes at all levels to its left to be less than its value, and same for the right. I tried using a bound, but I realize that it would not be possible to keep track of all the bounds.

---

## My Solution
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isValidBST(self, root: Optional[TreeNode]) -> bool:
        
        # inorder traversal
        arr = self.traverse(root, [])
        
        # check if it is always increasing
        for i in range(len(arr)-1):
            if arr[i] >= arr[i+1]:
                return False
            
        return True
    
    def traverse(self, root, arr):
        
        if root is None:
            return arr
        
        else:
        
            arr = self.traverse(root.left, arr)
            arr += [root.val]
            return self.traverse(root.right, arr)
```

### Idea

The idea is to traverse the array inorder (left, root, right) and convert it into an array. Then it would loop through the array and check if it was always increasing. 

### Complexity

Time: O(n) since we're going through all the elements in the array twice (for traversal and for checking if valid).
Space: O(n) for array to store the values

---

## Other solutions

```
def isValidBST(self, root: Optional[TreeNode]) -> bool:
        def helpDFS(root,cur_min,cur_max)-> bool:
            if not root:
                return True
            if root.val>=cur_max or root.val<=cur_min:
                return False
            isLeft = helpDFS(root.left, cur_min, root.val)
            isRight = helpDFS(root.right, root.val, cur_max)
            return isLeft and isRight
        return helpDFS(root, float('-inf'), float('inf'))
```

I guess this was what I was trying to do with the bounds. `isLeft` and `isRight` is checking if the new root will be between `cur_min` and `cur_max`. The values will be relatively to at which it is at.

My previous solution involved only looking at the root and having a single bounds and trying to implement a bunch of conditionals to see if it should be below or above the bound, if it was on the left side or right side. Instead of that, I could have just kept two values for min and max.

```python
class Solution:
    def find_max(self, root):
        if root.right is None:
            return root.val
        return self.find_max(root.right)
    def find_min(self, root):
        if root.left is None:
            return root.val
        return self.find_min(root.left)
    def isValidBST(self, root: Optional[TreeNode]) -> bool:
        if root is None:
            return True
        if root.left is not None and self.find_max(root.left) >= root.val:
            return False
        if root.right is not None:
            if self.find_min(root.right) <= root.val:
                return False
        if not self.isValidBST(root.left) or not self.isValidBST(root.right):
            return False
        return True
```

Same deal with comparing to a bound (`<= root.val`), this time the solution involved breaking the logic into helper functions. It looks like its finding the leftmost or rightmost of the binary tree and comparing it to the root

---

here's the new solution
```python
class Solution:
    def isValidBST(self, root: Optional[TreeNode]) -> bool:
        return self.recursiveCall(root, float('-inf'), float('inf'))
    
    def recursiveCall(self, root, curr_min, curr_max):
        
        if root is None:
            return True
        
        if root.val <= curr_min or root.val >= curr_max:
            return False
            
        return self.recursiveCall(root.left, curr_min, root.val) and \
    self.recursiveCall(root.right, root.val, curr_max)
                
```