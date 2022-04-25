# 284. Peeking Iterator

## Description

Design an iterator that supports the peek operation on an existing iterator in addition to the `hasNext` and the `next` operations.

Implement the `PeekingIterator` class:

- `PeekingIterator(Iterator<int> nums)` Initializes the object with the given integer iterator `iterator`.

- `int next()` Returns the next element in the array and moves the pointer to the next element.

- `boolean hasNext()` Returns `true` if there are still elements in the array.

- `int peek()` Returns the next element in the array **without** moving the pointer.

Note: Each language may have a different implementation of the constructor and Iterator, but they all support the `int next()` and `boolean hasNext()` functions.

## Classic Solution

### 1. Solution 1

```C++
/*
 * Below is the interface for Iterator, which is already defined for you.
 * **DO NOT** modify the interface for Iterator.
 *
 *  class Iterator {
 *		struct Data;
 * 		Data* data;
 *  public:
 *		Iterator(const vector<int>& nums);
 * 		Iterator(const Iterator& iter);
 *
 * 		// Returns the next element in the iteration.
 *		int next();
 *
 *		// Returns true if the iteration has more elements.
 *		bool hasNext() const;
 *	};
 */

class PeekingIterator : public Iterator {
private:
    int nextInt;
    
    int notEnd;
public:
	PeekingIterator(const vector<int>& nums) : Iterator(nums) {
	    // Initialize any member here.
	    // **DO NOT** save a copy of nums and manipulate it directly.
	    // You should only use the Iterator interface methods.
	    notEnd = Iterator::hasNext();
        if(notEnd)
            nextInt = Iterator::next();
	}
	
    // Returns the next element in the iteration without advancing the iterator.
	int peek() {
        return nextInt;        
	}
	
	// hasNext() and next() should behave the same as in the Iterator interface.
	// Override them if needed.
	int next() {
	    int res = nextInt;
        notEnd = Iterator::hasNext();
        if(Iterator::hasNext())
            nextInt = Iterator::next();
        return res;
	}
	
	bool hasNext() const {
	    return notEnd;
	}
};
```


### 2. Solution 2 - Copy Constructor
Since Iterator has a copy constructor, we can just use it.

In the code below, `Iterator(*this)` makes a copy of current iterator, then call next on the copied iterator to get the next value without affecting current iterator.

In addition, we needn't override `next()` and `hasNext()` method since they do exactly what base class do.

```C++
class PeekingIterator : public Iterator {
public:
	PeekingIterator(const vector<int>& nums) : Iterator(nums) {}
	
    // Returns the next element in the iteration without advancing the iterator.
	int peek() {
        return Iterator(*this).next(); 
	}
};
```