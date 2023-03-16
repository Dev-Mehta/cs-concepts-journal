# Hash Tables

A hash table, also known as hash map, is a data structure that implements an associative array or dictionary. 

It is an abstract data type that maps keys to values. A hash table uses a hash function to compute an index, also called a hash code, into an array of buckets or slots, from which the desired value can be found. 

During lookup, the key is hashed and the resulting hash indicates where the corresponding value is stored.

Mainly three operations are required to implement a hash table

- `insert(key, item)` - insert item at key or overwrite if already exists
- `delete(key)` - delete item with given key
- `search(key)` - return item with given key or report key error.

A hashmap basically contains set of items, each with a key. 

One way to implement hash tables is to use binary search trees like AVL Trees so you can do all of these operation in log(n) time. But you can do better.

We know so far that the best way to sort is `O(nlogn)` and best way to search is `O(logn)`.

Today we will go from searching from `O(logn)` to `O(1)` time. 

## Uses of hashtables

- We use them in docdist - counting words, how many times a word occurs in a document & computing inner products - finding common words between two documents. Its the best way to do things - easiest & fastest.
- database - when you go to Merriam webster and lookup for a word, when you spell check a word.
- Every search engine on the world would lookup for all documents from a specific keyword.(Pre-google)
- compilers & interpreters.(Variable names to address & memory). In the old days of python, it was in the interpreter; all the global and local variables were stored in the dictionary. They would match the keys to values. Now they don’t because its slow and there are better ways to do
- substring search
- string commonalities
- cryptography

## Simple approach

Direct-access table - store items in an array indexed by key(integer).

```jsx
[-]['hello'][-]['world'][-]
 0     1     2     3     4
```

### Limitations

1. Keys may not be integers
2. Its a gigantic memory hog

### Solution

1. Solution to 1 - prehash
    - Prehash functions maps key to non-neg. integers
    - In theory, keys are finite & discrete - We know anything on the computer can be written down as string of bits - So, we’re done. In theory this is easy. Atleast for analysis we will assume it is. In reality, its not quite so simple.
    - In python(2.7 and older)
        - hash(’\0B’) == hash(’\0\0C’) - hash(x) function returns prehash of x.
        - Its little tricky to find such examples but they are out there.
        - In an ideal world, this prehash function of x should be prehash function of y only when x == y. Sadly, in python its quite not true.
        - To find the key of custom objects you can implement `__hash__` or else it will use default function `id`.
2. The bigger problem is reducing space. So how do we do that?
    - Solution: hashing - To hash means to cut into pieces and mix around(**etymologically, not literally**).
    - We want to take all set of keys and we want to reduce them down to small set of integers.
    - But there is a catch here, we might run into collisions. To solve this problem we use different methods.

### Method 1 - Chaining

If you have multiple items of the same key’s hash(non-neg. number), store them as a linked list. That is hashing with chaining. 

The only question is why would you expect it to be any good?

- Worst case: You have a fancy data structure with a linked list disguised as hash-table. > O(n) time complexity.

But it can be proved that chaining is indeed a good method(under a given assumption).

#### Proof - Under an assumption
Simple uniform hashing assumption - In computer science, SUHA (Simple Uniform Hashing Assumption) is a basic assumption that facilitates the mathematical analysis of hash tables. 
	
The assumption states that a hypothetical hashing function will evenly distribute items into the slots of a hash table. 

Moreover, each item to be hashed has an equal probability of being placed into a slot, regardless of the other elements already placed. 

This assumption generalizes the details of the hash function and allows for certain assumptions about the stochastic system.

[See this for full proof](https://en.wikipedia.org/wiki/SUHA_(computer_science))

