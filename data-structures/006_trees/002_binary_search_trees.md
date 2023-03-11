# Binary Search Trees

A binary search tree, also called an ordered or sorted binary tree, is a rooted binary tree data structure with the key of each internal node being greater than all the keys in the respective node's left subtree and less than the ones in its right subtree.

The time complexity of operations on the binary search tree is directly proportional to the height of the tree.

| Operation | Average | Worst Case |
| ------------| ---------|--------------|
| Search | O(logn) | O(n) |
| Insert | O(logn) | O(n) |
| Delete | O(logn) | O(n) |

Binary search trees allow binary search for fast lookup, addition, and removal of data items. Since the nodes in a BST are laid out so that each comparison skips about half of remaining tree, the lookup performance is proportional to that of binary algorithm. 

The basic operations include search, insert, delete and traversal. The complexity analysis shows that we can achieve amortized `O(logn)` time complexity for search, insert and delete operations. In worst case, they degrade to that of a singly linked list - `O(n)` .

Binary search trees can be used to implement abstract data types such as dynamic sets, lookup tables, and priority queues, and used in sorting algorithms such as tree sort.

## Operations
### Searching

Searching in a binary tree for a specific key can be programmed both recursively and iteratively. 

```
## Recursive
fn search(root, key) {
	if root == null
		return does not exist
	if root->value == key then
		return root
	else if key < root->value
		return search(root->left, key)
	else
		return search(root->right, key)
}

## Iterative
fn search(root, key){
	while root != null and key != root->value
		if key < root->value
			root = root.left
		else
			root = root.right
	return root
}
```

Since, the search may proceed till some leaf node, the time complexity of search is `O(h)` where `h` is height of the tree. If the tree is [height-balanced-tree](#height-balanced-trees) then the height is `O(logn)`

### Insertion

Operations such as insertion and deletion cause the BST representation to change dynamically. The data structure must be changed in such a way that the properties of BST continue to hold. New nodes in BST are inserted as leaf-nodes.

```
fn insert(root, newnode){
	y = null
	x = root
	while x != null
		y = x
		if newnode->value < x->value
			x = x->left
		else
			x = x->right
	newnode->parent = y
	if y = null 
		root = newnode
	else if newnode->value < y->value
		y->left = newnode
	else
		y->right = newnode
}
```

This function maintains a "trailing pointer" y as a parent of x. After initialization on line 2, the while loop causes the pointers to be updated. If y is null, the BST is empty, thus newnode is inserted as the root node, if it is not null then insertion proceeds by comparing the values to that of y and the node is inserted accordingly on left or right side.

### Deletion

Deletion of a node D, from a tree should abide three cases:
	1. If D is a leaf node, the parent node's pointer gets replaced with null and consequently D gets removed from the tree.
	2. If D has a single child, the child gets elevated as either left or right child of D's parent depending on the position of D within the BST, and D gets removed from the tree.
	3. If D has both a left and right child, the successor of D takes the position of D in the tree. The successor of D let's say E depends on the position of E within the BST:
		a. If E is D's immidiate right child, E gets elevated and E's left child pointer is made to point D's initial left subtree 
		b. If E is not the immidiate right child of D, deletion proceeds by replacing the position of E by E's right child, and E takes the position of D in BST

```
fn delete(root, d){
	if d->left == null
		shift(root, d, d->right)
	else if d->right == null
		shift(root, d, d->left)
	else
		e = successor(d)
		if e->parent != d
			shift(root, e, e->right)
			e->right = d->right
			e->right->parent = e
		shift(root, d, e)
		e->left = d->left
		e->left->parent = e
}

fn shift(root, u, v){
	if u->parent == null
		root = v
	else if u = u->parent->left
		u->parent->left = v
	else
		u->parent->right = v
	if v == null
		v->parent = u->parent
}

fn successor(node){
	if node->right == null
		return minimum(x->right)
	y = node->parent
	while y != null and node == y->right
		node = y
		y = y.parent
	return y
}

fn minimum(node){
	while node->left != null
		node = node->left
	return node
}
```

### Traversal
#### In-Order
Nodes from left-subtree gets visited first, followed by root and then the right subtree

```
fn inorder(root){
	if  root != null
		inorder(root->left)
		## do something with node
		output root->value
		inorder(root->right)
}
```

#### Pre-order
The root node gets visited first, then the left subtree and then the right subtree

```
fn preorder(root){
	## do something with node
	if root != null
		output root->value
		preorder(root->left)
		preorder(root->right)
}
```

#### Post-order
The left subtree gets visited first, then the right subtree and then the root node

```
fn postorder(root){
	if root != null
		postorder(root->left)
		postorder(root->right)
		## do something with node
		output root->value
}
```

## Balanced BSTs

Without re-balancing, insertions or deletions of a BST, may lead to degeneration, resulting in height n of the tree, where n is the number of total nodes, which leads to lookup performance going down from O(logn) to O(n).

This can be achieved by self balancing mechanisms during the updation operations to the tree designed to maintain tree height to the binary logarithmic complexity.

### Height Balanced Trees

A tree is height balanced if the heights of the left sub-tree and right-subtree are guaranteed to be related by a constant factor. The heights of all the nodes on the path from the root to the modified leaf node have to be observed and possibly corrected on every insertion and deletion operation.

### Weight Balanced Trees

In a weight balanced tree, the criterion of a balanced tree is a number of leaves of the subtree. The weights of left and right subtree differ at most by 1.