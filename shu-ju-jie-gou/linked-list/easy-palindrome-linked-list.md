# \[Easy\] Palindrome Linked List

## [\[Easy\] Palindrome Linked List](https://leetcode.com/problems/palindrome-linked-list/) \(4568/420\)

Given a singly linked list, determine if it is a palindrome.

**Follow up:**  
Could you do it in O\(n\) time and O\(1\) space?

### 1. Copy into ArrayList & Compare Reversed: O\(N\) / O\(N\)

Determining whether or not an _Array List_ is a palindrome is straightforward. We can just use **reversed nums** to check \(`nums == nums[::-1]`\) whether nums is palindrome or not.  

因為需要完成一個nums ArrayList，我們只需要走一遍linked list，並且把每一個值加入到nums即可。  
`while head != None:  
    nums.append(head.val)      # 用head.val取值，並加到nums裡  
    head = head.next           # 往下走`

Time Complexity: O\(N\) where N is the number of nodes in the linked list. First it takes O\(N\) to traverse the linked list. Another O\(N\) is to compare the list. Total O\(2N\) -&gt; O\(N\).

```python
def isPalindrome(self, head: ListNode) -> bool:

    # reverse a linked list and compare it with the original one.
    nums = []
    curr_node = head
    
    # copy into ArrayList
    while curr_node != None:
        nums.append(curr_node.val)
        curr_node = curr_node.next
    
    # compare reversed
    return nums == nums[::-1]
```

要解決Follow-Up Question，就必須要用法二: Reverse Half of Linked List In-place。

### 2. Reverse Half of Linked List In-place: O\(N\) / O\(1\)



我們並不用擔心原始linked list數量是odd or even的問題。但為什麼呢？  
因為palindrome本身，odd版本最中間的item就可以不用對稱，因此我們可以

`head: 1->2->3->3->None  
            |`  
`reverse:    3->2->1->None`

```python
# e.g. 1->2->3->4->5->6->None   false
def isPalindrome(self, head: ListNode) -> bool:
    
    # 1->2->3->4->5->6->None 
    # reverse:       6->5->4->None
    #                      |
    # head:       1->2->3->4->None
    second_half_start = self.splitHalf(head)
    reverse = self.reverseHalf(second_half_start)
    
    # STEP3. compare
    # compare original first half (head) & reversed second half (reverse)
    # in order to see if it matches. 
    # reverse is going from the begining of second half
    # head is going from the begining of first half
    while reverse and head:
        if reverse.val != head.val:
            return False
        reverse = reverse.next
        head = head.next
    return True

# STEP1. split 
# fast&slow to get second half, slow is pointing to second half start
def splitHalf(self, head):  
    fast = slow = head
    while fast and fast.next:
        fast = fast.next.next
        slow = slow.next
    return slow

# STEP2. reverse
# reverse first half to get a reversed version of first half.
def reverseHalf(self, slow):       
    reverse = None
    while slow:
        next_node = slow.next
        slow.next = reverse
        reverse = slow
        slow = next_node     
    return reverse
```

