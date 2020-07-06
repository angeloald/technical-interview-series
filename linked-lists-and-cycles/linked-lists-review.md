# Linked Lists Review

## Quick Review

A linked list is a set of nodes that contain some data and references to other nodes. You will always get a reference to the head node.

### Singly-linked list

This is a linked list that lets you access all nodes by giving you access to the head node. You can find elements by traversing through the nodes, and you can add elements by reassigning the new element as the new head that points to the previous head. You can delete elements by traversing through the nodes and connecting the preceding node to the subsequent node.

It's easy to see that a singly-linked list can be used to implement a stack. You push items in and you pop them out the same way you'd add items into a linked list and remove items by controlling the head \(which acts as the top of the stack\).

Also, you can implement a queue by adding a reference to the tail of the singly-linked list. Enqueueing an element affects the tail node while dequeueing an element affects the head node.

### Doubly-linked list

This is almost like a singly-linked list, but its nodes can reference the two nodes adjacent to them. A doubly linked list adds more utility, at the cost of more memory to manage extra pointers, by giving you bidirectional traversal capabilities.

You can notice that a deque can be implemented as a doubly-linked list with an added reference to the tail node.

### Circular linked list

A circular linked list is either a singly-linked list or a doubly-linked list where the node that has a null reference also references the head node.

![http://pages.cs.wisc.edu/~skrentny/cs367-common/readings/Linked-Lists/index.html](../.gitbook/assets/image%20%284%29.png)

You can use a circular linked list to do many things like managing games where only one player can move at a time like Monopoly, creating circular buffers [which also happens to be what voice assistants use to listen to directives like "hey Siri"](http://www.freepatentsonline.com/y2019/0108839.html), and other things that involve cycling through the elements \(e.g. cycling through applications on your personal computer with `alt + tab` or `cmd + tab`\).

