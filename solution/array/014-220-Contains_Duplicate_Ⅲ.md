# 220. Contains Duplicate Ⅲ
## Description
Given an integer array `nums` and two integers `k` and `t`, return `true` if there are two distinct indices `i` and `j` in the array such that `abs(nums[i] - nums[j]) <= t` and `abs(i - j) <= k`.

## My Solution
My solution is sort of brute force. For each element `nums[i]`, it compare all the element with index `j` which satisfies `i + 1 <= j <= i + k`, need to mind the array boundary as well. However, it will receive a [TLE](https://leetcode.com/submissions/detail/603580648/) error in leetcode OJ. As a result, we'd better use value of element as bucket to compare, as is shown in the next section.

```C++
bool containsNearbyAlmostDuplicate(vector<int>& nums, int k, int t) {
    for(int i = 0; i < nums.size(); i++) {
        for(int j = i + 1; j < (i + k + 1 > nums.size() ? nums.size() : i + k + 1); j++) {
            long long value_i = nums[i];
            long long value_j = nums[j];
            if(abs(value_i - value_j) <= t)
                return true;
        }
    }
    return false;
}
```
## Classic Solution
Uses `std::multimap`, which is a kind of map, but enables existence of multiple equal key elements.
Note that to avoid overflow/underflow, we should use `long long` to store the value of elements.
```C++
bool containsNearbyAlmostDuplicate(vector<int>& nums, int k, int t) {
    multimap <int, int> dict;
    for(int i = 0; i < nums.size(); i++)
        dict.insert(pair<int, int>(nums[i], i));
    
    multimap <int, int>::iterator it, it_next;
    for(it = dict.begin(); it != dict.end(); it++) {
        it_next = it;
        while(1) {
            if(++it_next == dict.end())
                break;
            long long val_current = it->first;
            long long val_next = it_next->first;

            if(val_next - val_current <= t) {
                if(abs(it->second - it_next->second) <= k)
                    return true;
            }
            else
                break;        
        }
    }
    return false;
}
```
