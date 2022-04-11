# 341. Flatten Nested List Iterator


## Description

You are given a nested list of integers `nestedList`. Each element is either an integer or a list whose elements may also be integers or other lists. Implement an iterator to flatten it.

Implement the `NestedIterator` class:

- `NestedIterator(List<NestedInteger> nestedList)` Initializes the iterator with the nested list nestedList.
- `int next()` Returns the next integer in the nested list.
- `boolean hasNext()` Returns true if there are still some integers in the nested list and false otherwise.

Your code will be tested with the following pseudocode:
```pseudocode
initialize iterator with nestedList
res = []
while iterator.hasNext()
    append iterator.next() to the end of res
return res
```
If res matches the expected flattened list, then your code will be judged as correct.

**Example:**
```
Input: nestedList = [[1,1],2,[1,1]]
Output: [1,1,2,1,1]
Explanation: By calling next repeatedly until hasNext returns false, the order of elements returned by next should be: [1,1,2,1,1].
```

## My Solution
My implementation is not an iterator actually, but stores a copy of the result data with a current index.

```C++
/**
 * // This is the interface that allows for creating nested lists.
 * // You should not implement it, or speculate about its implementation
 * class NestedInteger {
 *   public:
 *     // Return true if this NestedInteger holds a single integer, rather than a nested list.
 *     bool isInteger() const;
 *
 *     // Return the single integer that this NestedInteger holds, if it holds a single integer
 *     // The result is undefined if this NestedInteger holds a nested list
 *     int getInteger() const;
 *
 *     // Return the nested list that this NestedInteger holds, if it holds a nested list
 *     // The result is undefined if this NestedInteger holds a single integer
 *     const vector<NestedInteger> &getList() const;
 * };
 */

class NestedIterator {
public:
    NestedIterator(vector<NestedInteger> &nestedList) {
        this->list.clear();
        this->index = -1;
        initializeList(nestedList);
    }
    
    void initializeList(vector<NestedInteger>& nestedList) {
        for(int i = 0; i < nestedList.size(); i++) {
            NestedInteger curr = nestedList[i];
            
            if(curr.isInteger())
                list.push_back(curr.getInteger());
            else 
                initializeList(curr.getList());
        }
    }
    
    int next() {
        if(hasNext()) {
            return list[++index];
        }
        return INT_MIN;
    }
    
    bool hasNext() {
        if(index + 1 < list.size())
            return true;
        return false;
    }
private:
    vector<int> list;
    
    int index;
};

/**
 * Your NestedIterator object will be instantiated and called as such:
 * NestedIterator i(nestedList);
 * while (i.hasNext()) cout << i.next();
 */
```

## Classic Solution

Here's a real iterator implementation from [StefanPochmann](https://leetcode.com/problems/flatten-nested-list-iterator/discuss/80146/Real-iterator-in-Python-Java-C%2B%2B).

In his opinion an iterator shouldn't copy the entire data (which some solutions have done) but just iterate over the original data structure.

I keep the current progress in a stack. My `hasNext` tries to find an integer. My `next` returns it and moves on. I call `hasNext` in `next` because `hasNext` is optional. Some user of the iterator might call only `next` and never `hasNext`, e.g., if they know how many integers are in the structure or if they want to handle the ending with exception handling.

```C++
class NestedIterator {
public:
    NestedIterator(vector<NestedInteger> &nestedList) {
        begins.push(nestedList.begin());
        ends.push(nestedList.end());
    }

    int next() {
        hasNext();
        return (begins.top()++)->getInteger();
    }

    bool hasNext() {
        while (begins.size()) {
            if (begins.top() == ends.top()) {
                begins.pop();
                ends.pop();
            } else {
                auto x = begins.top();
                if (x->isInteger())
                    return true;
                begins.top()++;
                begins.push(x->getList().begin());
                ends.push(x->getList().end());
            }
        }
        return false;
    }

private:
    stack<vector<NestedInteger>::iterator> begins, ends;
};
```