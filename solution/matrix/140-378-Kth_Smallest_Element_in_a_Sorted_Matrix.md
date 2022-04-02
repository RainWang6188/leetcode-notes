# 378. Kth Smallest Element in a Sorted Matrix

## Description
Given an `n x n` matrix where each of the rows and columns is sorted in ascending order, return the kth smallest element in the matrix.

Note that it is the kth smallest element in the sorted order, not the kth distinct element.

You must find a solution with a memory complexity better than $O(n^2)$.

**Example:**
```
Input: matrix = [[1,5,9],[10,11,13],[12,13,15]], k = 8
Output: 13
Explanation: The elements in the matrix are [1,5,9,10,11,12,13,13,15], and the 8th smallest number is 13
```

## Classic Solution

### 1. Binary Search
The key point for any binary search is to figure out the "Search Space". For me, I think there are two kind of "Search Space" -- index and range(the range from the smallest number to the biggest number). 

Most usually, when the array is sorted in one direction, we can use index as "search space", when the array is unsorted and we are going to find a specific number, we can use "range".

Let me give you two examples of these two "search space"

- index -- A bunch of examples -- https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/ ( the array is sorted)
- range -- https://leetcode.com/problems/find-the-duplicate-number/ (Unsorted Array)

The reason why we did not use index as "search space" for this problem is the matrix is sorted in two directions, we can not find a linear way to map the number and its index.


```C++
int kthSmallest(vector<vector<int>>& matrix, int k) {
    int m = matrix.size();
    int n = matrix[0].size();

    int low = matrix[0][0];
    int high = matrix[m-1][n-1];
    while(low < high) {
        int mid = low + ((high - low) >> 1);
        int count = 0;
        for(int i = 0; i < m; i++) {
            int j = n - 1;
            while(j >= 0 && matrix[i][j] > mid)
                j--;
            count += j + 1;
        }
        
        if(count >= k)
            high = mid;
        else
            low = mid + 1;
    }
    return low;
}
```

### 2. Heap
Similar idea to the question [Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/).

```C++
int kthSmallest(vector<vector<int>>& matrix, int k) {
    int m = matrix.size();
    int n = matrix[0].size();

    auto cmp = [](vector<int>& val1, vector<int>& val2) {
        return val1[0] > val2[0];  
    };
    priority_queue<vector<int>, vector<vector<int>>, decltype(cmp)> pq(cmp);
    
    for(int i = 0; auto& row : matrix) {
        pq.push({row[0], i, 0});
        i++;
    }
    
    k--;
    while(k--) {
        vector<int> currVal = pq.top();
        pq.pop();
        if(currVal[2] != n - 1) {
            int rowIndex = currVal[1];
            int colIndex = currVal[2] + 1;
            pq.push({matrix[rowIndex][colIndex], rowIndex, colIndex});
        }
    }
    return pq.top()[0];
}
```