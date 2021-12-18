# 219. Contains Duplicate Ⅱ
## Description

## My Solution
### 1. Brute Force
Using two pointers and costs $O(N^2)$ time, will receive a [TLE](https://leetcode.com/submissions/detail/603548857/) error.
```C++
bool containsNearbyDuplicate(vector<int>& nums, int k) {
    for(int i = 0; i < nums.size() - 1; i++) {
        for(int j = i + 1; j < nums.size(); j++) {
            if(nums[i] == nums[j] && abs(j - i) <= k)
                return true;
        }
    }   
    return false;
}
```

### 2. Hash Table
Since we need to compare the difference between indices when two elements are equal, so we can add index attribute into the hash map, simply replacing the original count by index vector. Everytime when we encounter an equivalence, we only need to compare the currrent index with the previous index, since they're cloest.
```C++
bool containsNearbyDuplicate(vector<int>& nums, int k) {
    unordered_map<int, vector<int>> dict;
    for(int i = 0; i < nums.size(); i++) {
        dict[nums[i]].push_back(i);
        int size = dict[nums[i]].size();
        if(size > 1 && abs(i - dict[nums[i]][size - 2]) <= k)
            return true;
    }
    return false;
}
```

## Classic Solution
Another version also uses `unordered_map`, however, it doesn't record all the index, only the previous index need to be recorded.
```C++
bool containsNearbyDuplicate(vector<int>& nums, int k) {
    unordered_map<int, int> dict;
    
    for(int i = 0; i < nums.size(); i++) {
        if(dict.count(nums[i]) == 0) {
            dict[nums[i]] = i;
        }
        else {
            if(i - dict[nums[i]] <= k)
                return true;
            else
                dict[nums[i]] = i;
        }
    }
    return false;
}
```