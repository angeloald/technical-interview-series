# Trees

## Introduction

A tree is just a collection of nodes where each node, except for the root node, is connected by a single parent node that can have multiple children nodes. The root node acts as the starting point for tree operations. A binary tree is the same thing as a tree but its nodes can only have up to two children. A binary search tree is a binary tree that follows these properties:

* the node's left child has a smaller value than the node
* the node's right child has a greater value than the node

Any binary tree should be able to do the following:

* `insert`
* `remove`
* `find`

You can implement a binary search tree for yourself to get more practice. A node that stores an integer value will look something like this:

```text
A valid binary search tree with x as the root will look like:

  4
 / \ 
2   6


class Node:
  integer value
  Node left
  Node right
  Node(value): value = value, left = None, right = None

x = Node(4)
y = Node(2)
z = Node(6)
x.left = y
x.right = z
```

## Traversals

### Depth-first search

Preorder, inorder, and postorder traversals are just ways of visiting every element in a binary tree. Depth-first means we're processing nodes as a path from a parent node up to a node that has no children, a leaf node. Once a leaf is reached, we backtrack to a point where we can traverse downwards again.

Here's a short English lesson to help you remember the different types of DFS:

* `pre-` is a prefix that means before something \(e.g. "preschool", "prepaid expense", "preclassical music"\). `preorder` traversal means we do something to the node before traversing to its children.
* `intra-` is a prefix that means within something \(e.g. "intracellular organelles", "intrapersonal intelligence"\). We say `inorder` rather than `intraorder` for convenience. `inorder`traversal means we traverse the left child, do something to the node, then traverse the right child.
* `post-` is a prefix that means after something \(e.g. "post-apocalyptic", "postcolonialism"\). `postorder` traversal means we traverse the left child, traverse the right child, then do something to the node.

#### Preorder traversal

```text
preorder(node):
  if node == None: return
  doStuff(node)
  preorder(node.left)
  preorder(node.right)
```

#### Inorder traversal

```text
inorder(node):
  if node == None: return
  inorder(node.left)
  doStuff(node)
  inorder(node.right)
```

#### Postorder traversal

```text
postorder(node):
  if node == None: return
  postorder(node.left)
  postorder(node.right)
  doStuff(node)

```

### Breadth-first search



