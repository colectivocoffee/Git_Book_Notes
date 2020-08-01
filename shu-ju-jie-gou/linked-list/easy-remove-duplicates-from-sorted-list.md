# \[Easy\] Remove Duplicates from Sorted List

[Remove Duplicates from Sorted List](https://leetcode.com/problems/remove-duplicates-from-sorted-list/)

```python
def deleteDuplicates(self, head: ListNode) -> ListNode:

    if head == None:
        return head
    
    curr = head
    while curr != None:
        #
        while curr.next != None and curr.val == curr.next.val:
            # skip duplicates
            # curr (1)->1->[2]->3->3
            #           x   |curr.next.next
            # curr (1)->2->3->3
            curr.next = curr.next.next
        # i += 1
        curr = curr.next
    
    return head

```

