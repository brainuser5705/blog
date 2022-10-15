# []()

## Problem Statement


---

## Thoughts


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

### Idea

1. Traverse the tree and for each node, store a list of its root nodes (starting from its immediate parent to the top root of the tree) into a map.
2. Once the map is filled, check if either p or q is the parent root by checking if it is in the other's list.
3. Otherwise, get the first common node of the p and q's list.

### Complexity

Time - O(n) for creating the map and going through the lists
Space - O(n * log n)

---

## Other solutions

