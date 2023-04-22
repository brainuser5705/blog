# Merge Two Sorted Linked List

My (recursive) solution to [the problem](https://leetcode.com/problems/merge-two-sorted-lists):
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def mergeTwoLists(self, list1: Optional[ListNode], list2: Optional[ListNode]) -> Optional[ListNode]:
        
        if list1 is None:
            return list2
        elif list2 is None:
            return list1
        else:
            val1 = list1.val
            val2 = list2.val

            if val1 < val2:
                list1.next = self.mergeTwoLists(list1.next,list2)
                return list1
            else:
                list2.next = self.mergeTwoLists(list2.next, list1)
                return list2
```

I don't why but I really overthought this problem. 

I actually originally solved this problem by in June but completely forgot. My previous solution was like the typical merge part of the merge sort algorithm (find the min value, make it `curr.next`, and keep doing that while `list1` and `list2` were not None). 

Now when I was trying to solve it again 3 months later, my head kept thinking "we gotta merge list1 into list2, and by merge that means INSERTION". So after checking whether to add it before or after a node, I would have to insert it (changing `next` property for both lists) and maintain two seperate lists at all times. This really complicated things because I still had to return the `head` node and there were a whole bunch of edge cases like if there were empty lists and so on.

This solution is simple, determine which list starts out first, then change its next value to the winner between its original next and the other list.