# 330. Patching Array
## Description
Given a sorted integer array `nums` and an integer `n`, add/patch elements to the array such that any number in the range `[1, n]` inclusive can be formed by the sum of some elements in the array.

Return the minimum number of patches required.

**example**
```
Input: nums = [1,3], n = 6
Output: 1
Explanation:
Combinations of nums are [1], [3], [1,3], which form possible sums of: 1, 3, 4.
Now if we add/patch 2 to nums, the combinations are: [1], [2], [3], [1,3], [2,3], [1,2,3].
Possible sums are 1, 2, 3, 4, 5, 6, which now covers the range [1, 6].
So we only need 1 patch.
```

## My Solution
I didn't figure out the correct solution... This problem can be solved by greedy as well according to [here](https://leetcode.com/problems/patching-array/discuss/78495/Share-my-thinking-process).


## Classic Solution
> This awesome solution is provided by [Stefan Pochman](https://leetcode.com/problems/patching-array/discuss/78488/Solution-%2B-explanation).

```C++
int minPatches(vector<int>& nums, int n) {
    long miss = 1; 
    int i = 0;
    int added = 0;
    
    while(miss <= n) {
        if(i < nums.size() && nums[i] <= miss)
            miss += nums[i++];
        else {
            miss += miss;
            added++;
        }
    }
    return added;
}
```

Let `miss` be the smallest sum in [0,n] that we might be missing. Meaning we already know we can build all sums in `[0,miss)`. Then if we have a number `num <= miss` in the given array, we can add it to those smaller sums to build all sums in `[0,miss+num)`. If we don't, then we must add such a number to the array, and it's best to add miss itself, to maximize the reach.

---

**Example:** Let's say the input is `nums = [1, 2, 4, 13, 43]` and `n = 100`. We need to ensure that all sums in the range `[1,100]` are possible.

Using the given numbers `1`, `2` and `4`, we can already build all sums from `0` to `7`, i.e., the range `[0,8)`. But we can't build the sum `8`, and the next given number (`13`) is too large. So we insert `8` into the array. Then we can build all sums in `[0,16)`.

Do we need to insert `16` into the array? No! We can already build the sum `3`, and adding the given `13` gives us sum `16`. We can also add the `13` to the other sums, extending our range to `[0,29)`.

And so on. The given `43` is too large to help with sum `29`, so we must insert `29` into our array. This extends our range to `[0,58)`. But then the `43` becomes useful and expands our range to `[0,101)`. At which point we're done.