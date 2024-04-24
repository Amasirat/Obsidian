Trees are a form of non-linear data structure where objects are organized in a hierarchical structure. In a mathematical sense, a tree is a [Graph](Graphs.md) that contains no circuits. 

The highest node of the tree is called the root and the lower nodes connected to it are its children.

![[tree-data-structure-terminologies.webp]]

## Some Terminology

* A node without any children is called a **leaf** node.
* The child of a child of a child....node is called that node's **descendant**. That node is also the **ancestor** of its descendants.
* Other nodes aside from **root** and **leaves** are called **internal nodes**.
* Nodes in the tree with same parent are **siblings**.
* each node is connected to other nodes via lines which are called **edges**.
* The edges between a source node to a destination node is a **Path**.
## Implementation

Trees are commonly implemented with a [Linked List](Linked Lists.md) data structure, with each node having pointers to their children.

```C
struct treeNode {
	T key;
	treeNode* left;
	treeNode* right;
	...
	...
};
```

There are a number of different types of tree in use for programming:

* [[Binary Trees]]
* [[AVL Trees]]
* [[Binary Search Tree]]