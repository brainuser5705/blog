# [Balanced Binary Tree](https://leetcode.com/problems/balanced-binary-tree/)

## Problem Statement

Given a binary tree, determine if it is height-balanced.

For this problem, a height-balanced binary tree is defined as:
a binary tree in which the left and right subtrees of every node differ in height by no more than 1.

---

## Thoughts

N/A

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
    def isBalanced(self, root: Optional[TreeNode]) -> bool:
        
        def get_height_count(node):
            
            if node is None:
                return 0, 0
            
            lh, rh = 0, 0
            if node.left:
                lh = 1
                lh += max(get_height_count(node.left))
            if node.right:
                rh = 1
                rh += max(get_height_count(node.right))
                
            return lh, rh
                
        # tree traversal (pre order)
        
        if root is None:
            return True
        
        else:
            if root.left and not self.isBalanced(root.left):
                    return False
                
            if root.right and not self.isBalanced(root.right):
                    return False
            
            lh, rh = get_height_count(root)
            return abs(lh - rh) <= 1
```

### Idea

#### **Main Algorithm**

The idea is to recursively traverse through the tree in a bottom-to-top approach.
We know that the tree is not balanced if a smaller subtree is not balanced, which is why there is this condition check:
```python
if root.left and not self.isBalanced(root.left):
    return False
    
if root.right and not self.isBalanced(root.right):
        return False
```
We keep doing that check until the root is None or the height count difference condition doesn't satisfy.

#### **Height Count Algorithm**

The return value of the algorithm is the left and right height/depth of a given node. It does that by recursively traversing its left and right child until it's a leaf node.Then it takes the *max* of the two height counts from its recursive calls and adds them to the final height count.

### Complexity

Some factorial? It goes through every node and for each it goes through every child node.

---

## Other solutions

### **Solution 1**
```python
class Solution:
    def isBalanced(self, root: Optional[TreeNode]) -> bool:
        if not root: return True
        ans = True
        def height(node):
            nonlocal ans
            if not node: return 0 # base case
            l = height(node.left)
            r = height(node.right)

            if abs(l-r)>1: ans = False
            return max(l,r)+1

        height(root)

        return ans
```
> [`nonlocal`](https://docs.python.org/3/reference/simple_stmts.html#grammar-token-python-grammar-nonlocal_stmt:~:text=and%20compile()%20functions.-,7.13.,-The%20nonlocal%20statement) - causes the listed identifiers to refer to previously bound variables in the nearest enclosing scope excluding globals.

- This one condenses the height count calculation and condition check into one algorithm. This makes sense since I will take the height counts and eventually check them against the condition. And rather than keeping two values separate, this one returns the max of the values.
- To separate the condition output from the computation of the heights in the same algorithm, this one uses `ans` to store the condition output.

**Solution 2** (faster)
```python
class Solution:
    def isBalanced(self, root: Optional[TreeNode]) -> bool:
        return self.height(root) != -1
        
    def height(self, root: Optional[TreeNode]) -> int:
        if root is None: # base case
            return 0
        
        if (lh := self.height(root.left)) == -1 or (rh := self.height(root.right)) == -1:
            return -1
        
        if abs(lh - rh) <= 1:
            return max(lh, rh) + 1
        
        return -1
```

- This one is the same idea except it condenses things even more.
    - Instead of using `ans` for boolean values, it uses `-1` as the return value
    - Premature ending with `lh := self.height(root.left)) == -1 or (rh := self.height(root.right)) == -1` conditional check, and following the root call with `!= -1`
- Uses the [walrus operator](https://medium.com/mlearning-ai/when-and-why-to-use-over-in-python-b91168875453#:~:text=This%20operator%20is%20used%20for%20and%20only%20for%20the%20assignment%20of%20variables%20within%20another%20expression.) to assign values in an expression

---

## Conclusion

- This is how to deal with two functions *(getting height and checking height)* in one. Killing two birds with one stone kind of deal.