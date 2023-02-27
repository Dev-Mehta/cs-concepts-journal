# Arrays

Arrays store elements in contiguous memory locations, resulting in easily calculable addresses for the elements stored and this allows faster access to an element at a specific index.

For example, an array of ten `32-bit (4-byte)` integer variables, with indices 0 through 9, may be stored as ten words at memory addresses 2000, 2004, 2008, ..., 2036, so that the element with index i has the address `2000 + (i × 4)`. The memory address of the first element of an array is called first address, foundation address, or base address. 

Because the mathematical concept of a matrix can be represented as a two-dimensional grid, two-dimensional arrays are also sometimes called "matrices". In some cases the term "vector" is used in computing to refer to an array, although tuples rather than vectors are the more mathematically correct equivalent. Tables are often implemented in the form of arrays, especially lookup tables; the word "table" is sometimes used as a synonym of array. 

Arrays are useful mostly because the element indices can be computed at run time. Among other things, this feature allows a single iterative statement to process arbitrarily many elements of an array. For that reason, the elements of an array data structure are required to have the same size and should use the same data representation.

## Limitations of Array

- Elements of array should represent same datatype.

- The number of elements should be fixed beforehand. 

### Why do we have these limitations?

#### 1. Arrays should be of fixed size, but why?

Arrays are of fixed size, which means that the number of elements that an array can hold is determined at the time of its creation and cannot be changed later.

1. Efficient memory management: When an array is created, the memory required to store all of its elements is allocated in contiguous memory locations. This allows for efficient memory management, as the computer can easily access any element in the array by calculating its memory address. If arrays were of variable size, it would be more difficult to manage memory efficiently.

2. Predictable performance: Because the memory locations used by an array are contiguous, accessing elements in the array can be done with a constant time complexity (O(1)). This means that the time it takes to access any element in the array is the same, regardless of its position in the array. If arrays were of variable size, the time complexity of accessing elements in the array would depend on the size of the array and the position of the element being accessed, which could lead to unpredictable performance.

3. Type safety: Fixed-size arrays are type-safe, meaning that the type of data stored in each element of the array is known at compile time. This allows the compiler to catch type-related errors before the program is run. If arrays were of variable size, it would be more difficult to ensure type safety, as the type of data stored in each element could change at runtime.

Let's say we have array of 3 integers, our program will allocate 3 slots of memory, and then we can populate it with data.

Now the memory of our computer is not just limited for one program. If we somehow populate the array with more than it can handle, it would result in memory leaks.

#### 2. Elements of array should represent same datatype

Let's take the example of an array that could hold elements of different datatypes such as int, boolean, and float, and see why this would not be possible.

```java
Object[] array = {1, true, "Hello"};
```

An int typically takes up 4 bytes of memory, a boolean typically takes up 1 byte of memory, and a float typically takes up 8 bytes of memory. 

If we were to combine all of these different datatypes into a single array, it would be difficult to manage the memory for each element, since they all have different sizes.

Another reason why this is not possible is that each datatype has different methods and properties that can be used with it. 

For example, we can perform mathematical operations on integers and floats, but not on boolean values. 

If we were to combine these different datatypes into a single array, it would be difficult to perform operations on the array that make sense for all of the different datatypes.

#### How javascript & python allow arrays with different datatypes?

The answer to that is the boxing of the size.

```javascript
const mixed = [10, "BMW", true];
```

Javascript is going to find the element with the maximum size in the array now in this case string is taking the maximum size of three bytes so it is going to take three bytes and allocate them for each of the element of the
array so in total it will allocate 9 bytes. 3 bytes per element in total for the
whole array.

So instead of 7 bytes(2-> int, 3->string, 1->boolean) we'll
have 9 bytes for this whole array. 

First we have the integer
instead of allocating 2 bytes since the maximum size of element in the array is
3 bytes so it is going to allocate 3. 

Same goes for the boolean
now when it comes the time to read the values from the memory
it knows that it has boxed all the elements to 3 bytes.

## Algorithmic Complexity

- `peek(index)` -> O(1)
- `first()` -> Return the first element -> O(1)
- `last()` -> O(1)
- `insertAt(index)` -> O(n)
- `deleteAt(index)` -> O(n)
- `updateAt(index)` -> O(1)
- `printAll()` -> Simply traversing through whole array -> O(n)

## Jagged Arrays

Jagged arrays are arrays within arrays where sub-arrays have different lengths. They are similar to multi dimensional arrays, but they don't have a proper order of column size.

For example, let's say we want to create an array that represents a matrix with different numbers of columns in each row. We could create a jagged array to represent this matrix as follows:

```java
int[][] arr = new int[3][];
arr[0] = new int[5];
arr[1] = new int[8];
arr[2] = new int[2];
```

```
arr[0] -> [][][][][]
arr[1] -> [][][][][][][][]
arr[2] -> [][]
```

Jagged arrays can be useful in situations where you need to represent data that has varying lengths or dimensions. 

For example, jagged arrays can be used to represent tables with varying numbers of columns, or graphs with varying numbers of edges. 

However, they can also be more complex to work with than regular arrays, since you need to keep track of the different lengths of each sub-array.

## Resources
- [Array Data Structure | Illustrated Data Structures](https://www.youtube.com/watch?v=QJNwK2uJyGs)
- Watch from 15m32s - [UC Berkeley CS61B - Linear and Multi-Dim Arrays](https://archive.org/details/ucberkeley_webcast_Wp8oiO_CZZE)
- [Jagged Arrays](https://www.youtube.com/watch?v=1jtrQqYpt7g)

## Implementation of Automatically Resizing Vector

```cpp
/** 
Task: To implement an automatically resizing vector
Functions:
	size() - number of items
	capacity() - number of items it can hold
	is_empty()
	at(index) - returns item at given index, blows up if index out of bounds
	push(item)
	insert(index, item) - inserts item at index, shifts that index's value and trailing elements to the right
	prepend(item) - can use insert above at index 0
	pop() - remove from end, return value
	delete(index) - delete item at index, shifting all trailing elements left
	remove(item) - looks for value and removes index holding it (even if in multiple places)
	find(item) - looks for value and returns first index with that value, -1 if not found
*/

#include <iostream>
using namespace std;

template<typename T> class Array{
	// arr is the integer pointer which stores the address of our vector
	T* arr;
	// capacity is the total storage available for the vector
	int _capacity;
	// current is the number of elements present in the vector
	int current;
	void resize(){
		if(current==_capacity){
			T* temp = new T[2 * _capacity];
			for(int i = 0; i < _capacity; i++){
				temp[i] = arr[i];
			}
			delete[] arr;
			_capacity *= 2;
			arr = temp;
		}
		if(current==_capacity/4){
			T* temp = new T[_capacity / 2];
			for(int i = 0; i < _capacity; i++){
				temp[i] = arr[i];
			}
			delete[] arr;
			_capacity = _capacity / 2;
			arr = temp;
		}
	}
	public: 
		Array(){
			arr = new T[1];
			_capacity = 1;
			current = 0;	
		}
		/**
		 * @return the number of items present in array 
		*/
		int size(){
			return current;
		} 
		/**
		 * @return the number of items array can hold 
		*/
		int capacity(){
			return _capacity;
		}
		/**
		 * @return whether the array is empty or not
		*/
		bool is_empty(){
			if(current==0)
				return true;
			return false;	
		}
		/**
		 * @param item - Adds item to the array
		*/
		void push(T item){
			if(current==_capacity){
				T* temp = new T[2 * _capacity];
				for(int i = 0; i < _capacity; i++){
					temp[i] = arr[i];
				}
				delete[] arr;
				_capacity *= 2;
				arr = temp;
			}
			arr[current] = item;
			current++;
		}
		/**
		 * @param index - index of item
		 * @return return the item at given index
		*/
		T at(int index){
			if(index < current){
				return arr[index];
			}
		} 
		/**
		 * @param index - index of item to be inserted
		 * @param item - item to be inserted
		*/
		void insert(int item, int index){
			// if index is equal to capacity
			// then this function is same as
			// push function defined earlier
			if(index == _capacity){
				push(item);
			}
			else{
				for(int i = this->current; i>=index; i--)
					arr[i] = arr[i-1];
				arr[index] = item;
				current++;
			}
		}
		/**
		 * @param item - item to be added
		*/
		void prepend(int item){
			insert(item, 0);
		}
		/**
		 * @return pops the last element and returns it
		*/
		T pop() {
			if(current > 0){
				current--;
				resize();
				return arr[current+1];
			}
		}
		/**
		 * Function to delete item at
		 * @param index index of the item to delete
		*/
		void deleteElement(int index){
			int temp = current;
			
			current--;
		}
		/**
		 * Function to remove all items that match given item
		 * @param item Item to remove. All matching items will be removed
		*/
		void remove(T item){
			const int s = this->size();
			for(int i = 0; i < s; i++){
				if(item == arr[i]){
					for(int j = i; j < s; j++){
						arr[j]=arr[j+1];
					}
					current--;
				}
			}
		}
		/**
		 * Function to find index of item
		 * @param item Item to look for
		*/
		int find(T item){
			int index = -1;
			for(int i = 0; i < current; i++){
				if(item == arr[i]){
					index = i;
					break;
				}
			}
			return index;
		}
		void print(){
			for(int i = 0; i < current; i++){
				cout << arr[i] << endl;
			}
		}
};

int main(){
	Array<int> array;
	return 0;
}
```
