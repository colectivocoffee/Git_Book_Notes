# \[Medium\] Odd Even Linked List

[Odd Even Linked List](https://leetcode.com/problems/odd-even-linked-list/)  
Given a singly linked list, group all odd nodes together followed by the even nodes. Please note here we are talking about the node number and not the value in the nodes.

You should try to do it in place. The program should run in O\(1\) space complexity and O\(nodes\) time complexity.

## Thought Process

      `original: 1->2->3->4->5->None  
desired output: 1->3->5->2->4->None`

在原來的Head上，新建兩個pointers odd/even，並且新建evenHead的pointer，來維持原始evenHead的位置，方便結束時接上odd tail。

```python
    # [Start]
    # head [(1)]->[2]->3->4->5->None
    #       odd  
    #            even
    #            eHead
    
    # [End]
    #       head (1)->3->[5]->None
    #                    odd  
    #   evenHead (2)->4->[None]     
    #                     even
    #           eHead
```

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
    # odd/even/evenHead are pointers to original 'head'
    # noted that odd will be the pointer on head
    # even will separate from odd once using odd.next
    # head (1)->2->3->4->5->None
    # odd  (1)->2->3->4->5->None
    # even    (2)->3->4->5->None
    odd = head 
    even = odd.next
    # create evenHead to attach later
    evenHead = even
    
    # use even instead of odd, because goest faster than odd here
    while even != None and even.next != None:
        # skip element
        # NOTE: odd goes first and then even, due to first element 1
        odd.next = odd.next.next
        even.next = even.next.next
        # head (1)->3->4->5->None
        # odd  (1)->3->4->5->None
        # even    (2)->4->5->None
        
        # move a step forward        
        odd = odd.next
        even = even.next
        # head (1)->3->4->5->None
        # odd     (3)->4->5->None
        # even       (4)->5->None
    
    #               odd
    #  head (1)->3->[5]->None
    #eHead (2)->4->None
    odd.next = evenHead
    
    return head

```

