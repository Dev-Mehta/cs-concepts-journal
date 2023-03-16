# Queues
- A queue is an ordered list of elements where elements are inserted at one end and removed from the other end.

- Queues follow a first-in, first-out (FIFO) principle, meaning the element that was inserted first will be the first one to be removed.

- The end where elements are inserted is called the rear, and the end where elements are removed is called the front.

- The process of inserting an element into the queue is called enqueue, and the process of removing an element from the queue is called dequeue.

- Queues can be implemented using arrays or linked lists.

- Queues are useful in many applications such as job scheduling, task management, message passing, and data buffering.

- Some variations of queues include circular queues and priority queues.

- Circular queues are queues where the front and rear ends are connected, forming a circle.

- Priority queues are queues where elements are removed in order of priority, which is assigned based on some criteria.

## Queue Operations

The operation of adding an element to the rear of the queue is known as enqueue, and the operation of removing an element from the front is known as dequeue. Other operations may also be allowed, often including a peek or front operation that returns the value of the next element to be dequeued without dequeuing it.

The operations of a queue make it a first-in-first-out (FIFO) data structure. In a FIFO data structure, the first element added to the queue will be the first one to be removed. This is equivalent to the requirement that once a new element is added, all elements that were added before have to be removed before the new element can be removed. 

## Amortized queue

This queue's data is stored in two singly-linked lists `f` and `r`. The list `f` holds the front part of the queue. The list `r` holds the remaining elements.(a.k.a, the rear of the queue) in _reverse order_. 

It is easy to insert the front of the queue by adding a node at the head of `f`. And if `r` is not empty, it is easy to remove from the end of the queue by removing the node at the head of `r`. When `r` is empty, the list `f` is reversed and assigned to `r` and then the head `r` is removed.

The insert(enqueue) always takes time `O(1)`. The removal takes `O(1)` when the list `r` is not empty. When `r` is empty, the reverse takes `O(n)` where `n` is the number of elements in `f`.But, we can say it is `O(1)` amortized time, because every element in `f` had to be inserted and we can assign a constant cost for each element in the reverse to when it was inserted.

## Real-time queues

The real-time queues achieves `O(1)` time for all operations, without amortization.

Real-time queues, also known as concurrent queues, are data structures that allow multiple threads to access and manipulate the queue simultaneously without interfering with each other.

In a real-time queue implemented with three linked lists, there are three separate lists that are used to store elements in the queue: the front list, the back list, and the middle list.

The front list holds the oldest elements in the queue, while the back list holds the newest elements. The middle list is used to connect the front and back lists and serves as a buffer for elements being added or removed from the queue.

When a new element is added to the queue, it is placed at the back of the middle list. If the middle list becomes too long, elements are moved from the middle list to the back list to maintain a balance between the front and back lists.

When an element is removed from the queue, it is taken from the front of the middle list. If the front list becomes too short, elements are moved from the front list to the middle list to maintain a balance between the front and back lists.

By using three separate linked lists, real-time queues can achieve constant-time access to the front and back of the queue, as well as constant-time insertions and deletions in the middle of the queue. This makes them a popular choice for high-performance systems that require efficient and concurrent access to queues.

## Circular Queue

```cpp
class Queue{
	private:
		
		int buf[5];   /* array to be treated as circular buffer of 5 integers */
		int end;       /* write index */
		int start;     /* read index */
	public:
		Queue(){
			start = end = 0;
		}
		void put(int item) {
			buf[end++] = item;
			end %= 5;
		}

		int get() {
			int item = buf[start++];
			start %= 5;
			return item;
		}
};
```

## Priority Queues

Priority queues are similar to regular queue or stack. Each element in a priority queue has an associated priority. In a priority queue, elements with high priority are served before elements with low priority.

While priority queues are often implemented using heaps, they are conceptually distinct from heaps. A priority queue is an abstract data structure like a list or a map; just as a list can be implemented with a linked list or with an array, a priority queue can be implemented with a heap or another method such as an unordered array.

### Operations

`is_empty()`, `insert_with_priority()`, `pull_highest_priority_element()`. 

In addition, peek (in this context often called find-max or find-min), which returns the highest-priority element but does not modify the queue, is very frequently implemented, and nearly always executes in `O(1)` time. This operation and its `O(1)` performance is crucial to many applications of priority queues.

More advanced implementations may support more complicated operations, such as `pull_lowest_priority_element`, inspecting the first few highest- or lowest-priority elements, clearing the queue, clearing subsets of the queue, performing a batch insert, merging two or more queues into one, incrementing priority of any element, etc.