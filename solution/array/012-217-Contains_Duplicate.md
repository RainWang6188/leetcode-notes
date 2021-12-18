# 217. Contains Duplicate
## Description
Given an integer array `nums`, return `true` if any value appears **at least twice** in the array, and return `false` if every element is distinct.

**example**
```
Input: nums = [1,2,3,1]
Output: true
```

## My Solution
### 1. Hash Table
Using hash table to record the occurance of each element, and return `true` if find any which appears twice, otherwise return `false`.
```C++
bool containsDuplicate(vector<int>& nums) {
    unordered_map<int, int> dict;
    
    for(int num : nums) {
        if(++dict[num] > 1) 
            return true;
    }
    return false;
}
```

### 2. Sort
First sort the array, and then check if there are any adjacent elements which are equal.
```C++
bool containsDuplicate(vector<int>& nums) {
    sort(nums.begin(), nums.end());
    
    for(int i = 0; i < nums.size() - 1; i++) {
        if(nums[i] == nums[i + 1])
            return true;
    }
    return false;
}
```

### 3. Set
> `std::set` is typically implemented as a binary search tree (RB tree is GCC 4.8). It costs $O(N\log N)$ to construct. 
>
> `std::unordered_map` uses hash table, $O(N)$ is expected.

`std::set` is a type of associative container in which each element has to be unique because the value of the element identifies it. The values are stored in a specific order. 

```C++
bool containsDuplicate(vector<int>& nums) {
    set<int> dict(nums.begin(), nums.end());
    return nums.size() > dict.size();
}
```