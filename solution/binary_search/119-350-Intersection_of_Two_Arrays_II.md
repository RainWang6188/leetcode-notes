# 119. Intersection of Two Arrays II

## Description
Given two integer arrays nums1 and nums2, return an array of their intersection. Each element in the result must appear **as many times as it shows in both arrays** and you may return the result in any order.

## My Solutionß

### 1. Using set_intersection
```C++
vector<int> intersect(vector<int>& nums1, vector<int>& nums2) {
    vector<int> res;
    sort(nums1.begin(), nums1.end());
    sort(nums2.begin(), nums2.end());
    set_intersection(nums1.begin(), nums1.end(), nums2.begin(), nums2.end(), back_inserter(res));
    return res;
}
```

### 2. Using unordered_map

```C++
vector<int> intersect(vector<int>& nums1, vector<int>& nums2) {
    vector<int> res;
    unordered_map<int, int> dict;
    for(auto& val : nums1)
        dict[val]++;
    
    for(auto& val : nums2) {
        if(dict[val]-- > 0)
            res.push_back(val);
    }
    return res;
}
```
