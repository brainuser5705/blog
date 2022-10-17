# [Lowest Common Ancestor of a Binary Search Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/)

## Problem Statement

Given a binary search tree (BST), find the lowest common ancestor (LCA) node of two given nodes in the BST.

According to the definition of LCA on Wikipedia: “The lowest common ancestor is defined between two nodes p and q as the lowest node in T that has both p and q as descendants (where we allow a node to be a descendant of itself).”

---

## Thoughts

N/A

---

## My Solution
```python
class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        
        m = {}
        self.traverseTree([], root, m)
        
        if q in m[p]:
            # check if q is parent root
            return q
        elif p in m[q]:
            # check if p is parent root
            return p
        else:
            # traverse through the ordered list and check if they have a root node in common
            p_len = len(m[p])
            q_len = len(m[q])
            
            if p_len <= q_len:
                for i in range(p_len):
                    if (m[p])[i] in m[q]:
                        return m[p][i]
                    
            elif q_len < p_len:
                for i in range(q_len):
                    if (m[q])[i] in m[p]:
                        return m[q][i]
            
        
    def traverseTree(self, arr, root, m):
        
        m[root] = arr
        
        if not(root.left is None and root.right is None):
            
            # have to make sure that the most recent root is first
            if root.left:
                self.traverseTree([root] + arr, root.left, m)
                
            if root.right:
                self.traverseTree([root] + arr, root.right, m)
```

### **Idea**

1. Traverse the tree and for each node, store a list of its root nodes (starting from its immediate parent to the top root of the tree) into a map.
2. Once the map is filled, check if either p or q is the parent root by checking if it is in the other's list.
3. Otherwise, get the first common node of the p and q's list.

### **Complexity**

Time - O(n) for creating the map and going through the lists
Space - O(n * log n)

---

## Other solutions

### **My solution 2**
```python
class Solution(object):
    def lowestCommonAncestor(self, root, p, q):
        """
        :type root: TreeNode
        :type p: TreeNode
        :type q: TreeNode
        :rtype: TreeNode
        """
    
        p_val = p.val
        q_val = q.val
        min_val = min(p_val, q_val)
        max_val = max(p_val, q_val)        
        
        def traverse(node):
            
            """
            special cases
            """
            if node == q or node == p:
                # if one of the node is encountered, then it should be the parent
                return node

            """
            recursive cases
            """
            if min_val < node.val and max_val > node.val:
                # nodes are on opposite sides
                return node
            elif min_val < node.val and max_val < node.val:
                # both less than node, go left
                return traverse(node.left)
            elif min_val > node.val and max_val > node.val:
                # both greater than node, go right
                return traverse(node.right)
            
        return traverse(root)
```

This solution uses the binary tree structure to its advantage. The crux of the solution is that we know that if `p` and `q` are on opposite sides of a node, then that node is the common ancestor. We can determine what sides `p` and `q` are in by checking its value against the node (as shown in *recursive cases*). 

The special case is not the typical `None` case (since that should never happen if `p` and `q` exist). It is instead checking if the node is either `p` or `q` and if so, then the root would be ancestor of the other node.

*Complexity:*
- Time: O(log n) - you're traversing down only one path of the binary tree
- Space: O(1) - no additional data structures

### **No special cases**
```python
class Solution(object):
    def lowestCommonAncestor(self, root, p, q):
        
        p_val=p.val
        q_val=q.val
        
        if p_val>root.val and q_val>root.val:
            return self.lowestCommonAncestor(root.right, p ,q)
        elif p_val<root.val and q_val<root.val:
            return self.lowestCommonAncestor(root.left, p ,q)
        else:
            return root
```

This solution doesn't have a special case, because the conditions already takes of it (`<` and `>` instead of `<=` and `>=`).

### **No recursion**
```python
class Solution(object):
    def lowestCommonAncestor(self, root, p, q):
        while root:
            if (p.val < root.val > q.val):
                root = root.left
            elif (p.val > root.val < q.val):
                root = root.right
            else:
                return root
```
- Since we will be traversing the tree until we find the node (and the node will never be `None`), we can update the root as we go instead of doing a recursive call/adding to the stack trace. 
- Also makes use of chain operators.