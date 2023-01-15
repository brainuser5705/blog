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