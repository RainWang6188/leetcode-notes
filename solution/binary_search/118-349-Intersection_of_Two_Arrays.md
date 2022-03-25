# 349. Intersection of Two Arrays

## Description
Given two integer arrays `nums1` and `nums2`, return an array of their intersection. Each element in the result must be unique and you may return the result in **any** order.
**Example:**
```
Input: nums1 = [4,9,5], nums2 = [9,4,9,8,4]
Output: [9,4]
Explanation: [4,9] is also accepted.
```

## My Solution
We can use [`std::set_intersection()`](https://en.cppreference.com/w/cpp/algorithm/set_intersection) to finish this task. Before using it, we need to first sort the two arrays.

**Note:** If we store the result in an array `arr`, we can use `back_inserter(arr)`. However, sicne `std::set` doesn't support this operations, we need to use `insertor(intersect, intersect.begin())` instead for sets.
```C++
vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
    unordered_set<int> intersect;
    sort(nums1.begin(), nums1.end());
    sort(nums2.begin(), nums2.end());
    
    set_intersection(nums1.begin(), nums1.end(), nums2.begin(), nums2.end(), inserter(intersect, intersect.begin()));
    
    vector<int> res;
    for(auto& val : intersect)
        res.push_back(val);
    return res;
}
```

## Classic Solution

### 1. Using unordered_set

```C++
vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
    if (nums1.empty() || nums2.empty()){
        return std::vector<int>();
    }
    std::unordered_set<int> set{nums1.cbegin(), nums1.cend()};
    std::vector<int> intersections;
    for (auto n: nums2){
        if (set.erase(n) > 0){ // if n exists in set, then 1 is returned and n is erased; otherwise, 0.
            intersections.push_back(n);
        } 
    }
    return intersections;
}
```

### 2. Binary Search
```C++
class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        sort(nums1.begin(), nums1.end());
        vector<int> res;
        unordered_set<int> intersect;
        for(auto& val : nums2) {          
            if(binarySearch(nums1, val))
                intersect.insert(val);
        }
        
        for(auto& val : intersect)
            res.push_back(val);
        
        return res;
    }
private:
    bool binarySearch(vector<int>& nums, int target) {
        int low = 0;
        int high = nums.size() - 1;
        while(low < high) {
            int mid = low + ((high - low) >> 1);
            if(nums[mid] < target)
                low = mid + 1;
            else
                high = mid;
        }
        return nums[low] == target;
    }
};
```