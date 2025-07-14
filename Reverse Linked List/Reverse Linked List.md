### STEP1
- まずは答えを見ずに5分ほど考える

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        current = head
        change_cnt = 0
        while current.next is not None:
            temp1 = current.val
            temp2 = current.next
            current.next = temp1
            current.val = temp2
            change_cnt += 1
            current = current.next
        for i in range(change_cnt-1, -1, 0):
            while current.next is not None:
                temp1 = current.val
                temp2 = current.next
                current.next = temp1
                current.val = temp2
                current = current.next
        return head
```

- 実行すると、AttributeError: 'int' object has no attribute 'next' が発生する

### STEP2

- 答えを確認
```python
class Solution:
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        # Base case: empty list or single node
        if head is None or head.next is None:
            return head
        
        # Recursively reverse the rest of the list
        new_head = self.reverseList(head.next)
        
        # Reverse the current node's pointer
        head.next.next = head
        head.next = None
        
        return new_head
```
- 個人的にはあまり直感的でないソースコードだなと思いつつ、シンプルな構造であることは間違いないので、答えをそのまま使用することにする

### STEP3
- 答えを見ずに3回記述

#### 1回目
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        if head is None or head.next is None:
            return head

        new_node = self.reverseList(head.next)

        head.next.next = head
        head.next = None

        return new_node
```

#### 2回目
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        if head is None or head.next is None:
            return head

        new_node = self.reverseList(head.next)

        head.next.next = head
        head.next = None

        return new_node
```

#### 3回目
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        if head is None or head.next is None:
            return head

        new_node = self.reverseList(head.next)

        head.next.next = head
        head.next = None

        return new_node
```
