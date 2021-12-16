# 229. Majority Element Ⅱ
## Description
Given an integer array of size `n`, find all elements that appear more than `⌊ n/3 ⌋` times.

**Example 1**
```
Input: nums = [3,2,3]
Output: [3]
```
**Example 2**
```
Input: nums = [1,2]
Output: [1,2]
```
## My Solution
I use the harsh table to record the number of each elements' occurrance.
```C++
vector<int> majorityElement(vector<int>& nums) {
    unordered_map<int, int> counter;
    vector<int> major;
    
    for(int num : nums) {
        counter[num]++;
    }
    
    unordered_map<int, int>::iterator it;
    for(it = counter.begin(); it != counter.end(); it++)
        if(it->second > nums.size() / 3) {
            major.push_back(it->first);
        }
    
    return major;
}
```

## Classic Solution: Boyer-Moore Majority Vote Algorithm
Boyer-Moore Majority Vote Algorithm is presented in this [paper](http://www.cs.rug.nl/~wim/pub/whh348.pdf). This algorithm uses $O(1)$ extra space and $O(N)$ time. 

In a voting scenario, the basic concept is **“Let’s cancel each others’ vote!”**. You keep a counter for the majority number `X`. If you find a number `Y` that is not `X`, the current counter should deduce 1. The reason is that if there is 5 `X` and 4 `Y`, there would be one (5-4) more `X` than `Y`. This could be explained as "4 `X` being paired out by 4 `Y`".

And since the requirement is finding the majority for more than ceiling of [n/3], the answer would be less than or equal to two numbers.
So we can modify the algorithm to maintain two counters for **two majorities**.
```python
# @param {integer[]} nums
# @return {integer[]}
def majorityElement(self, nums):
    if not nums:
        return []
    count1, count2, candidate1, candidate2 = 0, 0, 0, 1
    for n in nums:
        if n == candidate1:
            count1 += 1
        elif n == candidate2:
            count2 += 1
        elif count1 == 0:
            candidate1, count1 = n, 1
        elif count2 == 0:
            candidate2, count2 = n, 1
        else:
            count1, count2 = count1 - 1, count2 - 1
    return [n for n in (candidate1, candidate2)
                    if nums.count(n) > len(nums) // 3]
```