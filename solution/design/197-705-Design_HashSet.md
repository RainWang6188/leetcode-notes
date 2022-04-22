# 705. Design HashSet


## Description

Design a HashSet without using any built-in hash table libraries.

Implement `MyHashSet` class:

- `void add(key)` Inserts the value `key` into the HashSet.
- `bool contains(key)` Returns whether the value `key` exists in the HashSet or not.
- `void remove(key)` Removes the value `key` in the HashSet. If key does not exist in the HashSet, do nothing.
 

**Example:**
```
Input
["MyHashSet", "add", "add", "contains", "contains", "add", "contains", "remove", "contains"]
[[], [1], [2], [1], [3], [2], [2], [2], [2]]
Output
[null, null, null, true, false, null, true, null, false]

Explanation
MyHashSet myHashSet = new MyHashSet();
myHashSet.add(1);      // set = [1]
myHashSet.add(2);      // set = [1, 2]
myHashSet.contains(1); // return True
myHashSet.contains(3); // return False, (not found)
myHashSet.add(2);      // set = [1, 2]
myHashSet.contains(2); // return True
myHashSet.remove(2);   // set = [1]
myHashSet.contains(2); // return False, (already removed)
```

**Constraints:**

- `0 <= key <= 106`
- At most $10^4$ calls will be made to `add`, `remove`, and `contains`.

## My Solution

A dumb solution which uses the most memory.

```C++
class MyHashSet {
private:
	vector<bool> table;
public:
	MyHashSet() : table(1e6 + 1, false) {}
	
	void add(int key) {
		table[key] = true;
	}
	
	void remove(int key) {
		table[key] = false;
	}
	
	bool contains(int key) {
		return table[key];
	}
};
```

## Classic Solution

To implement a hashset data structure, here're some of the problems should be addressed.

- hash functino

    Can map any possible element in a collection to a fixed range of integer values ​​and store the element at the address corresponding to the integer value.

- collision resolution

    Since different elements may map to the same integer value, conflict handling is required when integer values ​​"collide". In general, there are several strategies for resolving conflicts:
    
    - chaining address

        Maintain a linked list for each hash value and put elements with the same hash value into this linked list.

    - open address

        When a conflict is found at the hash value `h`, according to a certain strategy, start from `h` to find the next non-conflicting position. For example, one of the simplest strategies is to constantly check the positions of the integers `h+1`, `h+2`, `h+3`...

    - rehash

        When a hash collision is found, another hash function is used to generate a new address.

- size expansion

    When there are too many elements in the hash table, the probability of collision will become larger and larger, and the efficiency of querying an element in the hash table will become lower and lower. Therefore, it is necessary to open up a larger space to alleviate the collision in the hash table.



```C++
class MyHashSet {
private:
    static const int base = 769;
    
    vector<list<int>> data;
    
    static int hash(int key) {
        return key % base;
    }
    
public:
    MyHashSet() : data(base) {
    }
    
    void add(int key) {
        if(contains(key))
            return;
        
        data[hash(key)].push_back(key);
    }
    
    void remove(int key) {
        int index = hash(key);
        for(auto it = data[index].begin(); it != data[index].end(); it++) {
            if(*it == key) {
                data[index].erase(it);
                return;
            }
        }
    }
    
    bool contains(int key) {
        int index = hash(key);
        for(auto it = data[index].begin(); it != data[index].end(); it++) {
            if(*it == key)
                return true;
        }
        return false;
    }
};

/**
 * Your MyHashSet object will be instantiated and called as such:
 * MyHashSet* obj = new MyHashSet();
 * obj->add(key);
 * obj->remove(key);
 * bool param_3 = obj->contains(key);
 */
```