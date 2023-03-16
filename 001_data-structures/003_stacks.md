# Stacks

A stack is a linear data structure that follows the Last-In-First-Out (LIFO) principle, where the last item added to the stack is the first one to be removed. It can be visualized as a stack of plates where each new plate is added to the top of the stack, and the only plate that can be removed is the one at the top.

Stacks can be implemented using arrays or linked lists. In the array implementation, the stack uses a fixed-size array and maintains a pointer to the top element of the stack. In the linked list implementation, each node in the list holds a data element and a pointer to the next node, and the top of the stack is represented by the head of the list.

A stack has two primary operations: push and pop. The push operation adds an element to the top of the stack, while the pop operation removes the top element from the stack.

In addition to push and pop, stacks also support a peek or top operation, which returns the top element of the stack without removing it.

Stacks can be used in many applications, including expression evaluation, backtracking, and recursion.

Stacks can also be used to implement other data structures, such as queues, by using two stacks in a specific way.

A stack can be implemented as a bounded stack, where the maximum size of the stack is fixed, or an unbounded stack, where the stack can grow dynamically.

When using a stack, it's important to keep track of the size to avoid stack overflow errors by checking if the stack is full before pushing a new element.

```cpp
#include <iostream>

using namespace std;

const int MAX_SIZE = 100; // Maximum size of the stack

class Stack {
	private:
		int top; // Index of the top element in the stack
		int data[MAX_SIZE]; // Array to store the elements
	public:
		Stack() {
			top = -1; // Initialize the top index to -1
		}

		// Check if the stack is empty
		bool isEmpty() {
			return top == -1;
		}

		// Check if the stack is full
		bool isFull() {
			return top == MAX_SIZE - 1;
		}

		// Push an element to the top of the stack
		void push(int value) {
			if (isFull()) {
				cout << "Error: Stack overflow" << endl;
				return;
			}
			top++;
			data[top] = value;
		}

		// Pop the top element from the stack
		void pop() {
			if (isEmpty()) {
				cout << "Error: Stack underflow" << endl;
				return;
			}
			top--;
		}

		// Return the top element of the stack
		int peek() {
			if (isEmpty()) {
				cout << "Error: Stack is empty" << endl;
				return -1;
			}
			return data[top];
		}

		// Print the elements of the stack
		void print() {
			cout << "Stack: ";
			for (int i = top; i >= 0; i--) {
				cout << data[i] << " ";
			}
			cout << endl;
		}
};

// Main function to test the stack implementation
int main() {
	Stack myStack;

	myStack.push(3);
	myStack.push(7);
	myStack.push(1);

	myStack.print(); // Output: Stack: 1 7 3

	myStack.pop();
	myStack.print(); // Output: Stack: 7 3

	int topValue = myStack.peek();
	cout << "Top value: " << topValue << endl; // Output: Top value: 7

	return 0;
}
```

## When to not use Stacks?
- When you need to perform operations that require a lot of searching: Stacks are not designed for searching operations, so if you need to perform a lot of searching on the elements in the stack, a different data structure may be more efficient.

- When you need to store large amounts of data: Stacks are typically implemented using arrays, which have a fixed size. If you need to store large amounts of data, a dynamic data structure such as a linked list may be more appropriate.

- When you need to implement parallel processing: Stacks are not well-suited for parallel processing, as they can only be accessed by a single thread at a time. Other data structures, such as queues or heaps, may be better suited for parallel processing.