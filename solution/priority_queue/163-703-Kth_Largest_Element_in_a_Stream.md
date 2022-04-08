# 703. Kth Largest element in a Stream

## Description

Design a class to find the `kth` largest element in a stream. Note that it is the `kth` largest element in the sorted order, not the `kth` distinct element.

Implement KthLargest class:

- `KthLargest(int k, int[] nums)` Initializes the object with the integer `k` and the stream of integers `1`.
- `int add(int val)` Appends the integer `val` to the stream and returns the element representing the `kth` largest element in the stream.
 
**Example:**
```
Input
["KthLargest", "add", "add", "add", "add", "add"]
[[3, [4, 5, 8, 2]], [3], [5], [10], [9], [4]]
Output
[null, 4, 5, 5, 8, 8]

Explanation
KthLargest kthLargest = new KthLargest(3, [4, 5, 8, 2]);
kthLargest.add(3);   // return 4
kthLargest.add(5);   // return 5
kthLargest.add(10);  // return 5
kthLargest.add(9);   // return 8
kthLargest.add(4);   // return 8
```

## My Solution

Using a min heap to store the stream, containing the first `k` largest elements, where the `kth` largest element is on the top of the heap.

However, when `k == 1`, the input initial stream may be empty, so we need to make sure there're current `k` elements in the heap before `pq->top()`.

```C++
class KthLargest {
public:
    KthLargest(int k, vector<int>& nums) {
        this->k = k;
        pq = new priority_queue<int, vector<int>, greater<int>>();
        for(auto& num : nums)
            pq->push(num);
        while(pq->size() > k)
            pq->pop();
    }
    
    int add(int val) {
        pq->push(val);
        if(pq->size() > k)
            pq->pop();
        return pq->top();
    }
    
    ~KthLargest() {
        delete pq;
    }
private:
    int k;
    priority_queue<int, vector<int>, greater<int>> *pq;
};

/**
 * Your KthLargest object will be instantiated and called as such:
 * KthLargest* obj = new KthLargest(k, nums);
 * int param_1 = obj->add(val);
 */
```