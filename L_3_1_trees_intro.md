# Lecture 03: Trees Introduction

> Adapted from materials from Drs. Bobeldyk and Rafiq.  
> Original slide image credit: Markus Krisetya on Unsplash.

## Table of Contents

- [Graphs](#graphs)
- [Trees](#trees)
- [Tree Terminology](#tree-terminology)
- [How Trees Are Stored](#how-trees-are-stored)
- [Binary Trees](#binary-trees)
- [Binary Search Trees](#binary-search-trees)
- [Tree Traversals](#tree-traversals)
- [BST Operations](#bst-operations)
- [Deleting from a BST](#deleting-from-a-bst)
- [Key Takeaways](#key-takeaways)

---

## Graphs

A **graph** is a mathematical structure made up of:

- **Vertices**, also called **nodes**
- **Edges**, which connect nodes

Nodes usually store data. In this lecture, assume each node has at least a **key**.

Edges:

- Always connect two nodes
- Can store data
- Can be **directed** or **undirected**

Example graph:

```mermaid
graph LR
    A((A)) --- B((B))
    A --- C((C))
    B --- D((D))
    C --- E((E))
    D --- E
```

Graphs can represent many real-world relationships, such as:

- Dialogue options in a game
- Rooms of a building
- Parent and offspring connections, such as genealogy or phylogeny
- Social networks

---

## Trees

A **tree** is a special type of graph.

### Requirements

A tree has:

- **No cycles**
- Exactly **one path** between any two given nodes

### Common Additions

Trees often also have:

- A **parent-child** structure
- A **root**, meaning the whole tree descends from one root node

Example rooted tree:

```mermaid
graph TD
    A((A)) --> B((B))
    A --> C((C))
    B --> D((D))
    B --> E((E))
    C --> F((F))
```

Trees can represent:

- Dialogue options in a game
- Rooms of a building
- Genealogy or phylogeny
- Social networks
- File systems and hierarchies

---

## Tree Terminology

Using this tree:

```mermaid
graph TD
    A((A)) --> B((B))
    A --> C((C))
    B --> D((D))
    B --> E((E))
    C --> F((F))
```

### Root

The **root** is the starting node of the tree.

In the example, `A` is the root.

### Parent and Child

A **parent** is a node directly above another node.

A **child** is a node directly below another node.

Example:

- `A` is the parent of `B` and `C`
- `B` and `C` are children of `A`

### Siblings

Nodes with the same parent are **siblings**.

Example:

- `B` and `C` are siblings because both have parent `A`

### Leaf Nodes

A **leaf node** is a node with no children.

In the example, `D`, `E`, and `F` are leaf nodes.

### Subtree

A **subtree** is a node and all of its descendants.

Example:

- `B` and all descendants of `B` form the left subtree of `A`

### Depth

The **depth** of a node is the number of edges between that node and the root.

Example:

- Depth of `A` is `0`
- Depth of `B` is `1`
- Depth of `D` is `2`

### Height

The **height** of a node is the maximum number of edges between that node and one of its descendant leaf nodes.

The **height of the tree** is the height of the root.

---

## How Trees Are Stored

There are multiple ways to store a tree.

### 1. Flat Representation

A tree can be stored as:

- A list or array of nodes with values
- A list of edges

Example:

```python
nodes = ["A", "B", "C", "D", "E", "F"]
edges = [
    ("A", "B"),
    ("A", "C"),
    ("B", "D"),
    ("B", "E"),
    ("C", "F"),
]
```

### 2. Recursive Data Structure

A common way to store trees is recursively. Each node stores:

- A key or value
- Pointers or references to its children

General tree node:

```python
class TreeNode:
    def __init__(self, key, value=None):
        self.key = key
        self.value = value
        self.children = []
```

Binary tree node:

```python
class BinaryTreeNode:
    def __init__(self, key, value=None):
        self.key = key
        self.value = value
        self.left = None
        self.right = None
```

---

## Binary Trees

**Binary** means **two**.

A **binary tree** is a tree where every node has **at most two children**.

Those children are usually called:

- The **left child**
- The **right child**

Example binary tree:

```mermaid
graph TD
    A((A)) --> B((B))
    A --> C((C))
    B --> D((D))
    B --> E((E))
    C --> F((F))
```

### Searching a Plain Binary Tree

If a binary tree has no ordering rule, searching it can take:

```text
O(n)
```

In the worst case, we may need to check every node.

---

## Binary Search Trees

A **binary search tree**, or **BST**, is a binary tree with one extra ordering rule.

### BST Property

For any node `X`:

1. `X.key` is greater than or equal to all keys in the left subtree of `X`
2. `X.key` is less than or equal to all keys in the right subtree of `X`

In short:

```text
left subtree <= node <= right subtree
```

This must be true for **every node** in the tree.

That also means every subtree must itself be a BST.

### Valid BST Example

```mermaid
graph TD
    N5((5)) --> N4((4))
    N5 --> N8((8))
    N4 --> N2((2))
    N8 --> N6((6))
    N8 --> N9((9))
```

This is a BST because:

- Everything left of `5` is `<= 5`
- Everything right of `5` is `>= 5`
- The same rule holds for every subtree

### Invalid BST Example

```mermaid
graph TD
    N5((5)) --> N4((4))
    N5 --> N8((8))
    N4 --> N2((2))
    N4 --> N6((6))
    N8 --> N9((9))
```

This is **not** a BST because `6` is in the left subtree of `5`, but `6 > 5`.

To fix it, move `6` into the right subtree of `5`, for example as the left child of `8`.

### A BST Can Be Unbalanced

```mermaid
graph TD
    N5((5)) --> N6((6))
    N6 --> N7((7))
    N7 --> N8((8))
    N8 --> N9((9))
```

This is still a BST, but it is shaped like a linked list.

The BST property is about **ordering**, not balance.

---

## Tree Traversals

A **tree traversal** is a method for visiting every node in a tree.

There are three main depth-first traversals:

1. **In-order walk**
2. **Pre-order walk**
3. **Post-order walk**

Use this BST for the examples:

```mermaid
graph TD
    N5((5)) --> N4((4))
    N5 --> N8((8))
    N4 --> N2((2))
    N8 --> N6((6))
    N8 --> N9((9))
```

### In-Order Walk

In-order visits nodes in this order:

```text
left subtree → node → right subtree
```

For the example tree:

```text
2, 4, 5, 6, 8, 9
```

Algorithm:

```python
def traverse_in_order(node):
    if node is not None:
        traverse_in_order(node.left)
        print(node.key)
        traverse_in_order(node.right)
```

For a BST, in-order traversal gives the keys in sorted order.

### Pre-Order Walk

Pre-order visits nodes in this order:

```text
node → left subtree → right subtree
```

For the example tree:

```text
5, 4, 2, 8, 6, 9
```

Algorithm:

```python
def traverse_pre_order(node):
    if node is not None:
        print(node.key)
        traverse_pre_order(node.left)
        traverse_pre_order(node.right)
```

Pre-order is useful for structures where the parent should be handled before children, such as:

- File hierarchies
- Abstract syntax trees

### Post-Order Walk

Post-order visits nodes in this order:

```text
left subtree → right subtree → node
```

For the example tree:

```text
2, 4, 6, 9, 8, 5
```

Algorithm:

```python
def traverse_post_order(node):
    if node is not None:
        traverse_post_order(node.left)
        traverse_post_order(node.right)
        print(node.key)
```

Post-order is useful when children must be handled before the parent, such as safe deletion.

---

## BST Operations

Use this BST for the examples:

```mermaid
graph TD
    N5((5)) --> N4((4))
    N5 --> N8((8))
    N4 --> N2((2))
    N8 --> N6((6))
    N8 --> N9((9))
```

### Find the Minimum

To find the minimum value in a BST:

> Always go left.

```python
def get_minimum(node):
    while node.left is not None:
        node = node.left
    return node
```

For the example tree, the minimum is:

```text
2
```

### Find the Maximum

To find the maximum value in a BST:

> Always go right.

```python
def get_maximum(node):
    while node.right is not None:
        node = node.right
    return node
```

For the example tree, the maximum is:

```text
9
```

### Find / Contains

To check whether a value is in a BST:

1. Start at the root
2. If the current node matches the target, return `True`
3. If the target is smaller than the current node, go left
4. If the target is larger than the current node, go right
5. If you cannot continue in the needed direction, the value is not in the tree

Example question:

```text
Does this tree contain 3?
```

Search path:

```text
5 → 4 → 2 → stop
```

Since `3` is larger than `2`, we would need to go right from `2`. But `2` has no right child, so `3` is not in the tree.

Algorithm:

```python
def contains(node, target):
    while node is not None:
        if target == node.key:
            return True
        elif target < node.key:
            node = node.left
        else:
            node = node.right

    return False
```

### Find a Successor

The **successor** of a node is the next largest value.

Example questions:

```text
What is the successor of 4?
What is the successor of 5?
```

For the example tree:

```text
successor(4) = 5
successor(5) = 6
```

Rules:

1. If the node has a right subtree, the successor is the minimum node in that right subtree.
2. If the node does not have a right subtree and it is a left child, the successor is its parent.
3. Otherwise, move upward until you find an ancestor with a larger key.

---

## Deleting from a BST

Deleting a node from a BST has several cases.

### Case 1: The Node Is a Leaf

If the node has no children:

1. Delete the node
2. Set the parent’s pointer to `None`

Example: deleting `9` from this tree:

```mermaid
graph TD
    N5((5)) --> N4((4))
    N5 --> N8((8))
    N4 --> N2((2))
    N8 --> N6((6))
    N8 --> N9((9))
```

Result:

```mermaid
graph TD
    N5((5)) --> N4((4))
    N5 --> N8((8))
    N4 --> N2((2))
    N8 --> N6((6))
```

### Case 2: The Node Has One Child

If the node has only one child:

1. Replace the node with its child
2. Connect the parent directly to that child

Example: deleting `4` when `4` only has child `2`:

```mermaid
graph TD
    N5((5)) --> N4((4))
    N5 --> N8((8))
    N4 --> N2((2))
    N8 --> N6((6))
    N8 --> N9((9))
```

Result:

```mermaid
graph TD
    N5((5)) --> N2((2))
    N5 --> N8((8))
    N8 --> N6((6))
    N8 --> N9((9))
```

### Case 3: The Node Has Two Children

If the node has two children:

1. Find the node’s successor
2. Replace the node with the successor
3. If the successor had a right child, move that child into the successor’s old position

Example: deleting `5` from this tree:

```mermaid
graph TD
    N5((5)) --> N4((4))
    N5 --> N8((8))
    N4 --> N2((2))
    N8 --> N6((6))
    N8 --> N9((9))
```

The successor of `5` is `6`.

Result:

```mermaid
graph TD
    N6((6)) --> N4((4))
    N6 --> N8((8))
    N4 --> N2((2))
    N8 --> N9((9))
```

If the successor has a right child, that right child replaces the successor’s old position.

Example before deleting `5`:

```mermaid
graph TD
    N5((5)) --> N4((4))
    N5 --> N8((8))
    N4 --> N2((2))
    N8 --> N6((6))
    N8 --> N9((9))
    N6 --> N7((7))
```

Result after replacing `5` with successor `6`:

```mermaid
graph TD
    N6((6)) --> N4((4))
    N6 --> N8((8))
    N4 --> N2((2))
    N8 --> N7((7))
    N8 --> N9((9))
```

---

## Key Takeaways

- A graph is made of nodes and edges.
- A tree is a graph with no cycles and exactly one path between any two nodes.
- A rooted tree has parent-child relationships starting from a root.
- A binary tree allows each node to have at most two children.
- A binary search tree adds an ordering rule: `left <= node <= right`.
- Searching a plain binary tree can take `O(n)` time.
- BST operations depend on the height of the tree.
- In-order traversal of a BST gives sorted values.
- BST deletion has three cases: leaf, one child, and two children.
