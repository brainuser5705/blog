#binary-tree

# [Leetcode - 110. Balanced Binary Tree](https://leetcode.com/problems/balanced-binary-tree/)

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

        def rec(node):
            if node is None:
                return 0
            else:

                # print("node:",node.val)

                ld = rec(node.left)
                rd = rec(node.right)

                # print("\tleft:",ld)
                # print("\tright:",rd)

                if ld != -1 and rd != -1 and abs(ld-rd) <= 1:
                    # print("\tdepth:", 1 + max(ld,rd))
                    return 1 + max(ld,rd)
                else:
                    # print("\tdepth:", -1)
                    return -1

        return rec(root) != -1
```

## Another Solution 

```python
class Solution:

    def isBalanced(self, root: Optional[TreeNode]) -> bool:

        def rec(node):

            if node is None:
                return 0

            if node.left is None and node.right is None:
                return 1

            l_h = rec(node.left)
            r_h = rec(node.right) 

            if abs(l_h-r_h) > 1 or l_h == -1 or r_h == -1 :
                return -1
            else:
                return 1 + max(l_h, r_h)

        return rec(root) != -1
```

Pretty much the same as the first solution except for the conditional. It ended up being the supposedly fastest and least memory-intensive solution.

## Main Idea

Binary trees are recursive in its strucure and we can use that recursive nature when computing its height. The base case would be a single node (with no children) with a height of 1. To compute the height recursively, you would take the max of the heights of the child nodes and add 1 (i.e. `1 + max(l_h, r_h)`).  

To "incorporate" the check of balance in the algorithm, we could set a "indicator" value (like `-1`) to signify that the balance was broken. The algorithm has a condition before it returns the height to perform that integerity check and return the indicator value if the check fails. As the recursion continues for the larger subgraphs, that "indicator" value would **propagate** until the final whole graph is computed for. In the algorithm, the propagation takes place in the condition (`l_h == -1 or r_h == -1`).

Finally at the end, we would check if the indicator value had been "set".

## Notes

I struggled with this problem for a while. I think the most confusing part was figuring out how to work with heights (integers) when the final outcome was a boolean. The indicator value is a good technique to employ in this situation.

---

## Another solution 2

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isBalanced(self, root: Optional[TreeNode]) -> bool:
        
        # find the max height on left side and right side, check if >1 difference
        # go until it is a left node
        # find subheight of both sides, and take the max out of them
        # traverse array, and see if the split is close
	    # get height count of each node encountered and check if the abs condition satisfy
        
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
        
        if root is not None:
            
            if root.left:
                if not self.isBalanced(root.left):
                    return False
                
            if root.right:
                if not self.isBalanced(root.right):
                    return False
            
            lh, rh = get_height_count(root)
            return abs(lh - rh) <= 1
        
        else:
            
            return True
```

This was my first successful algorithm. It is more deconstructed than the other solutions in that it computes the height values and the final boolean answer separately.

## Main Idea

In `get_height_count`, it computes the heights of the left and right child of a node recursively. In the main algorithm, it traverses the graph in pre-order while checking that the children are balanced. If any of the childs are unbalanced, then `False` is returned. The balance check compute the height count of the node and checks the difference.

Essentially, the height count is computed for every node that the traversal encounters. If node is found to be unbalanced, then the algorithm prematurely returns false and propagate that value to the root node.

This approach compared to the others is more top-bottom since it traverses the nodes from the root and has to reach the leaf nodes to start the checking/propagation process.