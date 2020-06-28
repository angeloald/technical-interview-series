# Heaps and Sorting

### 1. Merge Two Sorted Lists

### Problem

Merge two sorted linked lists.

### How we'll solve it

This is literally the same thing as the merge part of merge sort.

### Solution

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def mergeTwoLists(self, h1: ListNode, h2: ListNode) -> ListNode:
        
        # return early for special cases
        if h1 == None and h2 == None:
            return None
        if h1 == None:
            return h2
        if h2 == None:
            return h1
        
        # this is the merge part
        head = t1 = t2 = None
        if h1.val <= h2.val:
            head = h1
            t1 = h1.next
            t2 = h2
        else:
            head = h2
            t1 = h1
            t2 = h2.next
        
        # merge one by one
        temp = head
        while t1 != None and t2 != None:
            if t1.val <= t2.val:
                temp.next = t1
                t1 = t1.next
            else:
                temp.next = t2
                t2 = t2.next              
            temp = temp.next

        # append if one list is consumed
        if t2 == None:
            temp.next = t1
        else:
            temp.next = t2

        return head
```

## 2. Kth Largest Element in an Array

### Problem

Find the kth largest element by value in an unsorted array.

### How we'll solve it

This is a kind of problem where using a heap \(or a priority queue\) would help a lot. Remember that a heap will always have a reference to either the smallest \(min heap\) or largest \(max heap\) element at any time.

For this problem, we'll use a min heap \(since Python supports it by default\). We will convert the array into a heap and pop x items, where `x = heap_length - k` . The next item popped after that should be the kth largest element in the unsorted list.

### Solution

```python
import heapq

class Solution:
    def findKthLargest(self, nums: List[int], k: int) -> int:
        heapq.heapify(nums)
        for _ in range(len(nums)-k):
            heapq.heappop(nums)
        return nums[0]
```

## 3. Merge k Sorted Lists

### Problem

 Merge k sorted linked lists and return it as one sorted list.

### How we'll solve it

We'll use a heap again and we'll do the following:

1. Create a heap containing all of the node values of all of the linked lists.
2. Consume the heap by popping all of its elements.
3. When an element is popped, turn that value into a node and connect it to the preceding node.
4. If it is the first node, designate it as the output linked list head. 

### Solution

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

import heapq

class Solution(object):
    def mergeKLists(self, lists):
        heap = []

        # combine all into one iterable
        for list_head in lists:
            node = list_head
            while node != None:
                heappush(heap, node.val)
                node = node.next
        
        # return early if this is an empty heap
        if len(heap) == 0:
            return None
        
        # the first node will always be the smallest  
        cur_node = ListNode(heappop(heap))
        head = cur_node
        cur_node.next = None

        while len(heap) > 0:
            cur_node.next = ListNode(heappop(heap))
            cur_node = cur_node.next
            cur_node.next = None

        return head
```

