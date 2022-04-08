# 373. Find K Pairs with Smallest Sums

## Description

You are given two integer arrays `nums1` and `nums2` sorted in ascending order and an integer `k`.

Define a pair `(u, v)` which consists of one element from the first array and one element from the second array.

Return the k pairs `(u1, v1)`, `(u2, v2)`, ..., `(uk, vk)` with the smallest sums.

**Example 1:**
```
Input: nums1 = [1,7,11], nums2 = [2,4,6], k = 3
Output: [[1,2],[1,4],[1,6]]
Explanation: The first 3 pairs are returned from the sequence: [1,2],[1,4],[1,6],[7,2],[7,4],[11,2],[7,6],[11,4],[11,6]
```

## My Solution
Store the first largest `k` pairs in a max heap. If the sum of current pair is larger than the top of the heap, we can break the current loop.

```C++
vector<vector<int>> kSmallestPairs(vector<int>& nums1, vector<int>& nums2, int k) {
    auto cmp = [](pair<int, int>& p1, pair<int, int>& p2) {
        long sum1 = p1.first + p1.second;
        long sum2 = p2.first + p2.second;
        return sum1 < sum2;
    };
    priority_queue<pair<int, int>, vector<pair<int, int>>, decltype(cmp)> pq(cmp);
    
    for(int i = 0; i < nums1.size(); i++) {
        for(int j = 0; j < nums2.size(); j++) {
            long currSum = nums1[i] + nums2[j];
            if(pq.size() == k && currSum >= pq.top().first + pq.top().second)
                break;
            
            pq.push(make_pair(nums1[i], nums2[j]));
            if(pq.size() > k)
                pq.pop();
        }
    }
    
    vector<vector<int>> res;
    while(!pq.empty()) {
        res.push_back({pq.top().first, pq.top().second});
        pq.pop();
    }
    return res;
}
```

## Classic Solution

Kind of like k-way merge method. It is also used in [Merge K Sorted List](https://leetcode.com/problems/merge-k-sorted-lists/).

```C++
vector<vector<int>> kSmallestPairs(vector<int>& nums1, vector<int>& nums2, int k) {
    auto cmp = [](vector<int>& p1, vector<int>& p2) {
        long sum1 = p1[0] + p1[1];
        long sum2 = p2[0] + p2[1];
        return sum1 > sum2;
    };
    priority_queue<vector<int>, vector<vector<int>>, decltype(cmp)> pq(cmp);
    vector<vector<int>> res;
    
    for(int i = 0; i < nums1.size() && i < k; i++)
        pq.push({nums1[i], nums2[0], 0});
    
    while(!pq.empty() && k--) {
        vector<int> curr = pq.top();
        pq.pop();
        res.push_back({curr[0], curr[1]});
        if(curr[2] + 1 < nums2.size()) {
            int index = curr[2] + 1;
            pq.push({curr[0], nums2[index], index});
        }    
    }
    return res;
}
```