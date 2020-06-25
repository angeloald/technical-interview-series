# Linked Lists and Cycles

### 1. Linked List Cycle I

#### Problem

Given a linked list, determine if it has a cycle in it.

To represent a cycle in the given linked list, we use an integer `pos` which represents the position \(0-indexed\) in the linked list where tail connects to. If `pos` is `-1`, then there is no cycle in the linked list.

#### How we'll solve it

This is cycle detection in its purest form. We just need to keep track of two pointers that traverse the linked list at different speeds. If the linked list has a cycle, the two pointers will eventually collide. If there is no cycle, the faster pointer will reach the end of the linked list and the program terminates.

#### Solution

```python
class Solution:
    def hasCycle(self, head: ListNode) -> bool:
        # we can use the Floyd's turtle and hare algorithm which is actually just modulus arithmetic 
        p1 = head
        p2 = head
        
        while p1 and p2 and p2.next:
            p1 = p1.next
            p2 = p2.next.next
            if p1 == p2:
                return True
            
        return False
```

### 2. Linked List Cycle II

#### Problem

Given a linked list, return the node where the cycle begins. If there is no cycle, return `null`.

To represent a cycle in the given linked list, we use an integer `pos` which represents the position \(0-indexed\) in the linked list where tail connects to. If `pos` is `-1`, then there is no cycle in the linked list.

#### How we'll solve it

This is essentially going to use the same trick we used in the first cycle detection question. However, we need to find out where the cycle begins.

1. First, determine if there is a cycle. If there is no cycle, return `None`. If there is a cycle, let's find out where it is.
2.  Once the cycle is found, place one of the pointers back into the linked list's head.
3. Move both pointers forward one step at a time until they collide with each other.
4. Where they collide is where the cycle  begins. The proof for this involves some math, but you can verify that yourself \(hint: think about divisibility and some modular arithmetic, or just draw it on a piece of paper\).  

#### Solution

```python
class Solution:
    def detectCycle(self, head: ListNode) -> ListNode:
        # we can use the Floyd's turtle and hare algorithm which is actually just modulus arithmetic 
        p1 = head
        p2 = head
        
        cycle_detected = False
        
        while p1 and p2 and p2.next:
            p1 = p1.next
            p2 = p2.next.next
            
            # if cycle detected
            if p1 == p2:
                p1 = head
                cycle_detected = True
                break
        
        while cycle_detected:
            if p1 == p2:
                return p1 
            p1 = p1.next
            p2 = p2.next
            
        return None
```

### 3. Middle of the Linked List

#### Problem

Given a non-empty, singly linked list with head node `head`, return a middle node of linked list.

If there are two middle nodes, return the second middle node.

#### How we'll solve it

We just need to have two pointers where one pointer moves a step at a time while the other pointer moves at twice the speed. The middle is reached by the slower pointer when the faster pointer reaches the end of the list.

#### Solution 

```python
class Solution:
    def middleNode(self, head: ListNode) -> ListNode:
        p1 = head
        p2 = head
        
        while p2 and p2.next:
            p1 = p1.next
            p2 = p2.next.next
            
        return p1
```

### 4. Remove Nth Node From End of List

#### Problem

Given a linked list, remove the _n_-th node from the end of list and return its head.

#### How we'll solve it

1. We need to traverse the list once so we can identify its length.
2. Once we figure out how long the list is, we just traverse it again up to the `length - nth_node_from_end_of_the_list`

#### Solution

```python
class Solution:
    def removeNthFromEnd(self, head: ListNode, n: int) -> ListNode:
        size = 0
        p1 = head
        p2 = head
        
        # get length of list
        while p1:
            p1 = p1.next
            size += 1
        
        # return early if the nth node is the first node
        if size == n:
            return head.next
            
        # go to the (length - n)th node
        i = 1
        while i < size - n:
            p2 = p2.next
            i += 1
        
        # now is at the (length - n)th node
        p2.next = p2.next.next
            
        return head
```



