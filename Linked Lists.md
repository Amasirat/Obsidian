Linked list is a type of dynamic data structure in which each element contains a [Pointer](Pointers.md) to another element in the list. The elements here are called **nodes**.

Each node is a struct comprised of two types of things. one is the data or value and the other is a pointer or pointers to other nodes.

```C
struct node {
	T data;
	node* next;
};
```

The way we keep track of our list is having a 'head' pointer to the first node of the list.

There are many types of Linked lists:

* Singly Linked Lists: Linked lists that only have a one way pointer to next nodes.
* Doubly Linked Lists: Liked lists that have two way pointers to next and also their previous nodes.
* Circular Linked Lists: Where the last node points to the first node in the list.

One disadvantage this data structure has as opposed to [[Arrays]], is the fact that searching for specific nodes inside the list has a complexity of O(n) and there is no possibility of instant access to any nodes not at the start or end of the list.

However its advantages are its ability to append elements without the need to declare a fixed size of memory for the data structure.

# Algorithms

