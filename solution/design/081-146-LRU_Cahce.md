# 146. LRU Cache
## Description
Design a data structure that follows the constraints of a **Least Recently Used (LRU) cache**.

Implement the `LRUCache` class:

- `LRUCache(int capacity)` Initialize the LRU cache with **positive** size `capacity`.
- `int get(int key)` Return the value of the `key` if the `key` exists, otherwise return `-1`.
- `void put(int key, int value)` Update the `value` of the `key` if the `key` exists. Otherwise, add the `key-value` pair to the cache. If the number of keys exceeds the `capacity` from this operation, **evict** the least recently used key.

The functions get and put must each run in `O(1)` average time complexity.
## Classic Solution
The good thing about lists is that iterators are never invalidated by modifiers (unless erasing the element itself). This way, we can store the iterator to the corresponding LRU queue in the values of the hash map. Since using erase on a list with an iterator takes constant time, all operations of the LRU cache run in constant time.

The reason for a double-linked-list is fast removal. Doubly linked lists let us remove and insert in constant time if we have access to a node directly. The hashtable gives us access to a node directly.

If we use a singly linked list we will need to spend O(n) time to remove a node even if we have direct reference to the node that needs to get removed. (This is because to remove in a singly linked list we need to point nodeToDelete's previous node to nodeToDelete's next node. Finding nodeToDelete's previous is expensive if nodeToDelete is the last node in the list.)


```C++
class LRUCache {
private:
    list<pair<int, int>> m_list;        // m_list::iterator->first: key; second: value
    unordered_map<int, list<pair<int, int>>::iterator> m_map;   // m_map::iterator->first: key; second: m_list::iterator
    int _capacity;
public:
    LRUCache(int capacity) : _capacity(capacity) {}
    
    int get(int key) {
        unordered_map<int, list<pair<int, int>>::iterator>::iterator found_iter = m_map.find(key);
        if(found_iter == m_map.end())
            return -1;
        m_list.splice(m_list.begin(), m_list, found_iter->second);
        return found_iter->second->second;
    }
    
    void put(int key, int value) {
        unordered_map<int, list<pair<int, int>>::iterator>::iterator found_iter = m_map.find(key);
        if(found_iter != m_map.end()) {
            found_iter->second->second = value;
            m_list.splice(m_list.begin(), m_list, found_iter->second);
            return;
        }
        
        if(m_map.size() == _capacity) {
            int del_key = m_list.back().first;
            m_map.erase(del_key);
            m_list.pop_back();
        }
        
        m_list.emplace_front(make_pair(key, value));
        m_map[key] = m_list.begin();
    }
};

/**
 * Your LRUCache object will be instantiated and called as such:
 * LRUCache* obj = new LRUCache(capacity);
 * int param_1 = obj->get(key);
 * obj->put(key,value);
 */
```