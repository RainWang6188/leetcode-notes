# 1. Two Sum
## Description
Given an array of integers `nums` and an integer `target`, return indices of the two numbers such that they add up to `target`.

You may assume that each input would have **exactly one** solution, and you may not use the same element twice.

You can return the answer in any order.

**example**
```
Input: nums = [3,2,4], target = 6
Output: [1,2]
```

## My Solution
### 1. Brute Force
A naive solution is to use brute force to find the two elements whose sum is `target`.

Time Complexity: $O(N^2)$; Space Complexity: $O(1)$.

```C++
vector<int> twoSum(vector<int>& nums, int target) {
    vector<int> index;
    
    for(int i = 0; i < nums.size(); i++) {
        for(int j = i + 1; j < nums.size(); j++) {
            if(nums[i] + nums[j] != target)
                continue;
            else {
                index.push_back(i);
                index.push_back(j);
                return index;
            }
        }
    }
    return index;
}
```
### 2. Hash Map
Another time-saving comlexity is to use hashing, which only needs one pass.

We traverse the `nums` array linearly. For each element `nums[i]`, we first check if it's in the hash map. If not, we push `target - nums[i]` into the map, and set `i` as its value. So next time when we encounter `target - nums[i]`, we would easily to find that it has already in the hash map.

Time Complexity: $O(N)$; Space Complexity: $O(N)$.

```C++
vector<int> twoSum(vector<int>& nums, int target) {
    vector<int> index;
    unordered_map<int, int> dict;
    
    for(int i = 0; i < nums.size(); i++) {
        unordered_map<int, int>::iterator it = dict.find(nums[i]);
        if(it == dict.end())
            dict[target - nums[i]] = i;
        else {
            index.push_back(it->second);
            index.push_back(i);
            return index;
        }
    }
    return index;
}
```
