# \[Easy\] Merge Two Sorted Lists

## [\[Easy\] Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/)    \(7165/795\)

Merge two sorted linked lists and return it as a **sorted** list. The list should be made by splicing together the nodes of the first two lists.

![](../../.gitbook/assets/image%20%28103%29.png)

```python
def mergeTwoLists(self, l1: ListNode, l2: ListNode) -> ListNode:
    
    # l1 Linked List -> List
    list_1 = []
    while l1:
        list_1.append(l1.val)
        l1 = l1.next
    
    # l2 Linked List -> List
    list_2 = []
    while l2:
        list_2.append(l2.val)
        l2 = l2.next
    
    # sort merged list
    merged = sorted(list_1 + list_2)
    
    # convert back: List -> Linked List
    pointer = dummy = ListNode(0)
    for num in merged:
        pointer.next = ListNode(num)
        pointer = pointer.next
    
    return dummy.next
```

