# \[Medium\] Add Two Numbers

## [\[Medium\] Add Two Numbers](https://leetcode.com/problems/add-two-numbers/)      \(11393/2709\)

![](../../.gitbook/assets/image%20%2881%29.png)

```text
Input: l1 = [2,4,3], l2 = [5,6,4]
Output: [7,0,8]
Explanation: 342 + 465 = 807.
```

Each node contains a single digit and the digits are stored in reverse order.

### 1. Division, Modulous + DummyNode : O\(max\(m,n\)\) / O\(max\(m,n\)\)

![Visualization of the addition of two numbers: 342 + 465 = 807](../../.gitbook/assets/image%20%2885%29.png)

由於linked list `l1` & `l2`都是reversed order，這跟我們正常加法一樣。  
從個位數開始加，再十位數，再百位數，....  
但我們需要一個變量 `carry` 來紀錄下一位是否要+1。carry只可能是`1 or 0`。

```python
def addTwoNumbers(self, l1: ListNode, l2: ListNode) -> ListNode:

    result = ListNode(0)
    # result_tail is used to move the pointer, 
    # whereas result remains at the begining. 
    tail_pointer = result
    # keypoint: use CARRY to update next level 
    carry = 0

    while l1 or l2 or carry:
        # get values from l1 and l2 
        val1 = l1.val if l1 else 0
        val2 = l2.val if l2 else 0
        
        # add two numbers
        curr_sum = carry + val1 + val2
        # update carry
        carry = curr_sum // 10
        
        # create a new node
        tail_pointer.next = ListNode(curr_sum%10)  #得到餘數
        # move the pointer
        tail_pointer = tail_pointer.next

        # move l1 and l2            
        l1 = l1.next if l1 else None
        l2 = l2.next if l2 else None

    return result.next
```

或者是用**divmod** `a, b = divmod(curr_sum,10)` 

```python
place holder
```

### 2. Yield:   O\(max\(m,n\)\) / O\(max\(m,n\)\)

STEP1. Convert the linked lists to integers, perform the addition  
STEP2. Convert the sum back to a linked list.

```python
def addTwoNumbers(self, l1: ListNode, l2: ListNode) -> ListNode:
    
    # convert link list to integers, 
    # and then perform the addition.
    def decode(n: ListNode):
        i = 1
        while n is not None:
            yield n.val * i
            i = 10 * i
            n = n.next
    
    # convert back to a linked list 
    def encode(i: int):
        temp = None
        for val in str(i):
            node = ListNode(val)
            node.next = temp
            temp = node
        return node
    
    i1 = sum(decode(l1))
    i2 = sum(decode(l2))
    
    return encode(i1 + i2)
```

## Follow Up

What if the the digits in the linked list are stored in **non-reversed order**? For example: $$(3 \to 4 \to 2) + (4 \to 6 \to 5) = 8 \to 0 \to 7$$ 

