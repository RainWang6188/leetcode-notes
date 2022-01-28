# 347. Top K Frequent Elements

## Description
Given an integer array `nums` and an integer `k`, return the `k` most frequent elements. You may return the answer in any order.

**Example:**
```
Input: nums = [1,1,1,2,2,3], k = 2
Output: [1,2]
```
## My Solution
At first, I thought about using `std::map` and sort it according to the order of values. However, it seems that `std::map` doesn't support this operation.

As a result, I store the `pair<int, int>`, where the first parameter is the element in `nums` and the second parameter is the occurance of that value, in a max heap sorted according to the descending occurance, and pop the top `k` elements.
```C++
vector<int> topKFrequent(vector<int>& nums, int k) {
    vector<int> res;
    unordered_map<int, int> dict;
    for(auto num : nums)
        dict[num]++;
    
    auto cmp = [&](pair<int, int> a, pair<int, int> b) {
        return a.second < b.second;
    };
    unordered_map<int, int>::iterator it = dict.begin();
    priority_queue<pair<int, int>, vector<pair<int, int>>, decltype(cmp)> pq(cmp);
    for(it = dict.begin(); it != dict.end(); it++) 
        pq.push(make_pair(it->first, it->second));
    
    for(int i = 0; i < k; i++) {
        auto cur = pq.top();
        pq.pop();
        res.push_back(cur.first);
    }
    return res;
}
```

## Classic Solution
### 1. MinHeap
Actually, we can use a min heap which stores the most frequent `k` elements at the top.

We also seperate the case when `k == N` to ensure the time complexity $O(N\log k)$ is less than $O(N\log N)$.

```C++
vector<int> topKFrequent(vector<int>& nums, int k) {
    // O(1) time
    if (k == nums.size()) {
        return nums;
    }

    // 1. build hash map : element and how often it appears
    // O(N) time
    map<int, int> count_map;
    for (int n : nums) {
        count_map[n] += 1;
    }

    // initialise a heap with most frequent elements at the top
    auto comp = [&count_map](int n1, int n2) { return count_map[n1] > count_map[n2]; };
    priority_queue<int, vector<int>, decltype(comp)> heap(comp);

    // 2. keep k top fequent elements in the heap
    // O(N log k) < O(N log N) time
    for (pair<int, int> p : count_map) {
        heap.push(p.first);
        if (heap.size() > k) heap.pop();
    }

    // 3. build an output array
    // O(k log k) time
    vector<int> top(k);
    for (int i = k - 1; i >= 0; i--) {
        top[i] = heap.top();
        heap.pop();
    }
    return top;
}
```
### 2. Quickselection
Quickselect is a textbook algorthm typically used to solve the problems "find kth something": kth smallest, kth largest, kth most frequent, kth less frequent, etc. Like quicksort, quickselect was developed by Tony Hoare, and also known as Hoare's selection algorithm.

It has $\mathcal{O}(N)$ average time complexity and is widely used in practice. It worth noting that its worst-case time complexity is $\mathcal{O}(N^2)$, although the probability of this worst-case is negligible.

The approach is the same as for quicksort.

> One chooses a pivot and defines its position in a sorted array in a linear time using so-called partition algorithm.

As an output, we have an array where the pivot is on its perfect position in the ascending sorted array, sorted by the frequency. All elements on the left of the pivot are less frequent than the pivot, and all elements on the right are more frequent or have the same frequency.

Hence the array is now split into two parts. If by chance our pivot element took N - kth final position, then kk elements on the right are these top kk frequent we're looking for. If not, we can choose one more pivot and place it in its perfect position.


If that were a quicksort algorithm, one would have to process both parts of the array. That would result in $\mathcal{O}(N \log N)$ time complexity. In this case, there is no need to deal with both parts since one knows in which part to search for N - kth less frequent element, and that reduces the average time complexity to $\mathcal{O}(N)$.

**Algorithm**

The algorithm is quite straightforward :

- Build a hash map element -> its frequency and convert its keys into the array unique of unique elements. Note that elements are unique, but their frequencies are not. That means we need a partition algorithm that works fine with duplicates.

- Work with unique array. Use a partition scheme (please check the next section) to place the pivot into its perfect position pivot_index in the sorted array, move less frequent elements to the left of pivot, and more frequent or of the same frequency - to the right.

- Compare pivot_index and N - k.

    -   If pivot_index == N - k, the pivot is N - kth most frequent element, and all elements on the right are more frequent or of the same frequency. Return these top kk frequent elements.

    - Otherwise, choose the side of the array to proceed recursively.


**Lomuto's Partition Scheme**

There is a zoo of partition algorithms. The most simple one is Lomuto's Partition Scheme, and so is what we will use in this article.

Here is how it works:

- Move pivot at the end of the array using swap.

- Set the pointer at the beginning of the array store_index = left.

- Iterate over the array and move all less frequent elements to the left swap(store_index, i). Move store_index one step to the right after each swap.

- Move the pivot to its final place, and return this index.

```C++
class Solution {
private:
    vector<int> unique;
    map<int, int> count_map;

public:
    int partition(int left, int right, int pivot_index) {
        int pivot_frequency = count_map[unique[pivot_index]];
        // 1. move pivot to the end
        swap(unique[pivot_index], unique[right]);

        // 2. move all less frequent elements to the left
        int store_index = left;
        for (int i = left; i <= right; i++) {
            if (count_map[unique[i]] < pivot_frequency) {
                swap(unique[store_index], unique[i]);
                store_index += 1;
            }
        }

        // 3. move pivot to its final place
        swap(unique[right], unique[store_index]);

        return store_index;
    }

    void quickselect(int left, int right, int k_smallest) {
        /*
        Sort a list within left..right till kth less frequent element
        takes its place. 
        */

        // base case: the list contains only one element
        if (left == right) return;

        int pivot_index = left + rand() % (right - left + 1);

        // find the pivot position in a sorted list
        pivot_index = partition(left, right, pivot_index);

        // if the pivot is in its final sorted position
        if (k_smallest == pivot_index) {
            return;
        } else if (k_smallest < pivot_index) {
            // go left
            quickselect(left, pivot_index - 1, k_smallest);
        } else {
            // go right
            quickselect(pivot_index + 1, right, k_smallest);
        }
    }

    vector<int> topKFrequent(vector<int>& nums, int k) {
        // build hash map : element and how often it appears
        for (int n : nums) {
            count_map[n] += 1;
        }

        // array of unique elements
        int n = count_map.size();
        for (pair<int, int> p : count_map) {
            unique.push_back(p.first);
        }

        // kth top frequent element is (n - k)th less frequent.
        // Do a partial sort: from less frequent to the most frequent, till
        // (n - k)th less frequent element takes its place (n - k) in a sorted array.
        // All element on the left are less frequent.
        // All the elements on the right are more frequent.
        quickselect(0, n - 1, n - k);
        // Return top k frequent elements
        vector<int> top_k_frequent(k);
        copy(unique.begin() + n - k, unique.end(), top_k_frequent.begin());
        return top_k_frequent;
    }
};
```