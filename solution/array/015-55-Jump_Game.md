# 55. Jump Game
## Description
You are given an integer array `nums`. You are initially positioned at the array's first index, and each element in the array represents your maximum jump length at that position.

Return `true` if you can reach the last index, or `false` otherwise.

**example 1**
```
Input: nums = [2,3,1,1,4]
Output: true
Explanation: Jump 1 step from index 0 to 1, then 3 steps to the last index.
```

**example 2**
```
Input: nums = [3,2,1,0,4]
Output: false
Explanation: You will always arrive at index 3 no matter what. Its maximum jump length is 0, which makes it impossible to reach the last index.
```

## My Solution
At first, I thought about using greedy algorithm, always finding the farthest index at each position and to see if we can finally reach the last index. However, it would return false when encounter testcase like `nums = [5,9,3,2,1,0,2,3,3,1,0,0]`, which is wrong since we can get to the end if not using greedy algorithm.

### 1. Backtracking
After some thoughts, it came to me that we can start from the end and record the farthest index we can reach, backtracking to the front. If the first index records its farthest index is the last index, it means it can reach the end.

```C++
bool canJump(vector<int>& nums) {
    int max_index_allowed = nums.size() - 1;
    vector<int> reach(nums.size(), 0);

    for(int i = nums.size() - 1; i >= 0; i--) {
        int max_index = min<int>(i + nums[i], max_index_allowed);

        for(int j = i + 1; j <= min<int>(i + nums[i], max_index_allowed); j++) {
            max_index = max<int>(max_index, reach[j]);
            if(max_index == max_index_allowed)
                break;
        }

        reach[i] = max_index;
    }
    
    return reach[0] == max_index_allowed;
}
```

### 2. BFS
Using BFS to check if the we can reach the last index. However, it will receive a [TLE](https://leetcode.com/submissions/detail/604010015/) error.
```C++
bool canJump(int nums[], int n) {
    bool res = false;

    int start = 0;
    if(start + nums[start] >= n - 1)
        return true;
    
    for(int i = start + 1; i <= min<int>(start + nums[start], n - 1); i++) {
        res |= canJump(nums + i, n - i);
    }

    return res;
}
```

## Classic Solution
### Greedy 
Also use greedy algorithm, but not as me, it iterates each index and updates the maximum index that can be reached so far.
```c++
bool canJump(vector<int>& nums) {
    int max_index = 0;
    for(int i = 0; i <= min<int>(nums.size() - 1, max_index); i++) {
        max_index = max<int>(max_index, nums[i] + i);
        if(max_index >= nums.size() - 1)
            return true;
    }
    return false;
}
```