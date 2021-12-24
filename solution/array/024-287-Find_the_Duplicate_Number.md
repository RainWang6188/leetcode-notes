# 287. Find the Duplicate Number
## Description
Given an array of integers `nums` containing `n + 1` integers where each integer is in the range `[1, n]` inclusive.

There is only **one repeated number** in `nums`, return this repeated number.

You must solve the problem **without** modifying the array `nums` and uses only constant extra space.

**example**
```
Input: nums = [1,3,4,2,2]
Output: 2
```

## My Solution
My solution uses $O(N)$ extra memory. I use the `unordered_set` to remove the duplicates, then we can find the duplicate number from difference between the sum of the original vector `nums` and the sum of `unordered_set`.
```C++
int findDuplicate(vector<int>& nums) {
    unordered_set<int> dict(nums.begin(), nums.end());
    long dict_sum = 0;
    for(int val : dict) 
        dict_sum += val;
    
    long nums_sum = 0;
    for(int num : nums)
        nums_sum += num;
    
    return (nums_sum - dict_sum) / (nums.size() - dict.size());
}
```


## Classic Solution
### 1. Binary Search + Pigeonhole Principle
There're `n+1` elements ranging in `[1, n]`, so there must be at least two elements which are equal. Although the value is not ordered, the indices are still ordered, so we can employ index to implement the binary search.

In each iteration, the algorithm finds the number of objects that should be filled into the first `n/2` holes.

1. If this number` k >= n/2+1`, then this is complies to pigeonhole principle again, we search this subproblem.

2. otherwise, consider the remaining `n/2` holes and remaining objects to fill. Because `k <= n/2`, the number of remaining objects is `r=(n+1)-k>=n/2+1`, that is, the remaining objects and remaining `n/2` holes complies to pigeonhole principle again, we just need to search this subproblem.

Time Complexity: $O(N\log N)$. Space Complexity: $O(1)$
```C++
int findDuplicate(vector<int>& nums) {
    int low = 0, high = nums.size() - 1;
    
    while(low < high) {
        int mid = low + (high - low) / 2;
        int sum = 0;
        for(int num : nums)
            if(num <= mid)
                sum++;
        
        if(sum <= mid)
            low = mid + 1;
        else if(sum > mid)
            high = mid;
    }
    
    return low;
}
```

### 2. Floyd's Tortoise and Hare Algorithm
If there is no duplicate in the array, we can map each indexes to each numbers in this array. In other words, we can have a mapping function `f(index) = number`

For example, let's assume
`nums = [2,1,3]`, then the mapping function is `0->2, 1->1, 2->3`.

If we start from `index = 0`, we can get a value according to this mapping function, and then we use this value as a new index and, again, we can get the other new value according to this new index. We repeat this process until the index exceeds the array. Actually, by doing so, we can get a sequence. Using the above example again, the sequence we get is `0->2->3`. (Because `index=3` exceeds the array's size, the sequence terminates!)

However, if there is duplicate in the array, the mapping function is many-to-one.

For example, let's assume
`nums = [2,1,3,1]`, then the mapping function is `0->2, {1,3}->1, 2->3`. Then the sequence we get definitely has a cycle. `0->2->3->1->1->1->1->1->........` The starting point of this cycle is the duplicate number.

We can use Floyd's Tortoise and Hare Algorithm to find the starting point.

Here is the detail of Floyd's Tortoise and Hare Algorithm.

1. Initialize two pointers (tortoise and hare) that both point to the head of the linked list
2. Loop as long as the hare does not reach null
3. Set tortoise to next node
4. Set hare to next, next node
5. If they are at the same node, the list is circular. 
7. Reset the tortoise back to the head.
6. Have both tortoise and hare both move one node at a time until they meet again
7. Return the node in which they meet
8. Else, if the hare reaches null, then return null

You can have a proof of the algorithm [here](https://leetcode.com/discuss/general-discussion/1116359/Intro-to-Floyd's-Cycle-Detection-Algorithm).

Time Complexity: $O(N)$, Space Complexity: $O(1)$.
```c++
int findDuplicate(vector<int>& nums) {
    int tortoise = 0, hare = 0;
    
    do{
        tortoise = nums[tortoise];
        hare = nums[nums[hare]];
    } while(tortoise != hare);
    
    tortoise = 0;
    while(tortoise != hare) {
        tortoise = nums[tortoise];
        hare = nums[hare];
    }
    
    return tortoise;
}
```