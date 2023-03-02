# Linked Lists

## Linked List Basics
### Why Linked Lists?

Linked lists and arrays are similar since they both store collections of data. The terminology is that arrays and linked lists store "elements" on behalf of "client" code.

The specific type of element is not important since essentially the same structure works to store elements of any type.

One way to look at how arrays work & think about alternate approaches.

#### Array Review

Arrays are the most common data structure used to store collections of elements.

The following code allocates an array `int scores[100]`, sets the first three elements set to contain numbers 1,2,3 and leaves the rest uninitialized.

```cpp
void arrayTest(){
	int scores[100];
	scores[0] = 1;
	scores[1] = 2;
	scores[2] = 3;
}
```

Here is a graphic, of how the scores array might look like in memory. 

```
scores: [1]--->[2]--->[3]--->[-3452]---...--->[24501]
index:   0      1      2        3      ..        99
```

Once the array is setup access to any element is convenient and fast with the [] operator(The address of the element is computed as an offset from the start of the array which only requires one multiplication & one addition - offset + (index*size)).

**The disadvantages of arrays are**:
1. The size of the array is fixed. With a little extra effort, the size of array can be deffered by dynamically allocating an array in an heap and then resizing with it `realloc()`, but that requires some real programmer effort.

2. Because of point (1), the most convenient thing for programmers to do is to allocate arrays which seem "large enough". This has two disadvantages: 
	- Most of the times the extra memory is wasted.
	- If program ever needs more memory than allocated, the code breaks.

3. Inserting new elements at front is expensive.

Linked lists have their own strengths and weaknesses, but they happen to be strong where arrays are weak.

## What linked lists look like?

Linked list allocates space for each element seperately in its own block of memory, called a "node". The list is overall structures by using pointers or references to link together the nodes in a chain.

Each node contains two fields: "data" & "next", where next field stores address of next node, and data - obviously data. 

```
singly_linked_list
------------------

list: [{data: 1, next: #01ab} ---> {data: 2, next: #02ab} ---> {data: 3, next: null}]
address:     #0001            --->         #01ab          --->         #02ab
```
### Example

```cpp
struct Node {
	int data;
	Node* next;
};

class LinkedList{
	private:
		Node* head;
	public:
		LinkedList() { head = nullptr; }

		void addFirst(int data){
			Node* newNode = new Node;
			newNode->data = data;
			newNode->next = head;
			head=newNode;
		}
		void printList() {
            Node* current = head; // start at the beginning of the list
            while (current != nullptr) {
                cout << current->data << " ";
                current = current->next; // move to the next node
            }
            cout << endl;
        }
};
```

## When should you not use linked lists.

1. Random access: If you need to access elements of the list randomly, linked lists can be inefficient. Linked lists don't provide constant-time access to elements in the middle of the list; you have to traverse the list from the beginning until you reach the desired element.

2. Large memory overhead: Linked lists require extra memory for each node in the list. This can make them inefficient for small lists and impractical for large lists.

3. Insertion or deletion at arbitrary positions: While linked lists excel at insertion or deletion at the beginning or end of the list, they can be slow if you need to insert or delete nodes at arbitrary positions. This is because you have to traverse the list to find the appropriate position.

4. Cache performance: Linked lists can have poor cache performance due to their non-contiguous memory allocation. This can be a problem in applications that require frequent traversal of the list.

5. Serialization: Linked lists are not easily serializable, which can be a problem if you need to store or transmit the list over a network or between different processes.

**For points 2, 3 & 4**

Consider a situation where you want to search for position of element in array:

```
for element in array
	if element is_equal_to k 
		return element.index
# We are making one simple operation
# Access address -> check data -> repeat until found
```

Whereas in linked list it would go something like this

```
counter = 0
while node not null
	if node.data is_equal_to k
		return counter
	else
		counter = counter + 1
		node.address = node.next.address
# We are making more operations than array
# Access address of data -> Check data -> Access address of next node -> Manipulate address of current node -> Repeat until found
```

In general, linked lists are best suited for applications that require frequent insertion or deletion at the beginning or end of the list, and where random access is not required. 

If you need random access or have other requirements that make linked lists unsuitable, you may want to consider using a different data structure, such as an array, a tree, or a hash table.