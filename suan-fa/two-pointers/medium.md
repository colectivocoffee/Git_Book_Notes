# \[Medium\]

[Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)  
Given a linked list, remove the _n_-th node from the end of list and return its head.

#### Example

```text
Given linked list: 1->2->3->4->5, and n = 2.

After removing the second node from the end, the linked list becomes 1->2->3->5.
```

## Thought Process

1. **Move Pointers:** move fast pointers forwarded by n steps
2. **Check Null:** check if fast has `fast.next`, if not then return `head.next`
3. **Move Pointers:** move both slow&fast pointers until fast pointers reach at the end of the list
4. **Perform the Action**: Delete Nth node
5. return head

## Code

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
def removeNthFromEnd(self, head: ListNode, n: int) -> ListNode:
        
        ### Key Idea: Use fast&slow pointers to move at the same time ###
        ### if fast reaches to the end, then slow is at the Nth node. ###
        # init
        slow = fast = head
        # move fast forward by n
        for _ in range(n):
            fast = fast.next
        
        # fast is the last one
        if fast.next is None:
            return head.next
        
        while fast.next != None:
            slow = slow.next
            fast = fast.next
        # delete Nth node
        slow.next = slow.next.next
        
        return head
```

