# \[Medium\] Convert Array -&gt; BST / Convert Linked List -&gt; BST

## \[Medium\] Convert Sorted List to BST      \(2758/94\)

### 1. DFS Recursive, Preorder + Binary Search

```python
# Utitlity method 1 to convert original linked list -> array
def convert(self, head):
    converted_list = []
    while head:
        converted_list.append(head.val)
        head = head.next
    return converted_list

# Utility method 2 to convert array -> BST
def listToBST(self, converted, start, end):

    if start > end:
        return

    middle = start + (end - start) // 2
    curr = TreeNode(converted[middle])
    if start == end:
        return curr

    curr.left = self.listToBST(converted, start, middle - 1)
    curr.right = self.listToBST(converted, middle + 1, end)
    return curr

# linked list --> array -binary search--> BST
def sortedListToBST(self, head: ListNode) -> TreeNode:

    if not head:
        return None

    converted = self.convert(head)
    root = self.listToBST(converted, 0, len(converted) - 1)

    return root
```

