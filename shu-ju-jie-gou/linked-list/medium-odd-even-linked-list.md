# \[Medium\] Odd Even Linked List

[Odd Even Linked List](https://leetcode.com/problems/odd-even-linked-list/)  
Given a singly linked list, group all odd nodes together followed by the even nodes. Please note here we are talking about the node number and not the value in the nodes.

You should try to do it in place. The program should run in O\(1\) space complexity and O\(nodes\) time complexity.

## Thought Process

## Code

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
def oddEvenList(self, head: ListNode) -> ListNode:
    if head == None:
        return head
    
    odd = head 
    even = odd.next
    # create evenHead to attach later
    evenHead = even
    
    # use even instead of odd
    while even != None and even.next != None:
        # skip
        # NOTE: odd goes first and then even, due to first element 1
        odd.next = odd.next.next
        even.next = even.next.next
        # move a step forward        
        odd = odd.next
        even = even.next
        
    odd.next = evenHead
    return head

```

