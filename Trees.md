Trees are a form of non-linear data structure where objects are organized in a hierarchical structure. In a mathematical sense, a tree is a [Graph](Graphs.md) that contains no circuits. 

The highest node of the tree is called the root and the lower nodes connected to it are its children.

![[tree-data-structure-terminologies.webp]]

## Some Terminology

* A node without any children is called a **leaf** node.
* The child of a child of a child....node is called that nodes **descendant**. That node is also the **ancestor** of its descendants.
* Other nodes aside from **root** and **leaves** are called **internal nodes**.
* Nodes in the tree with same parent are **siblings**.


## Implementation

Trees are commonly implemented with a [Linked List](Linked Lists.md) data structure, with each node having pointers to their children.

```C
struct treeNode {
	T key;
	treeNode* left;
	treeNode* right;
};
```
