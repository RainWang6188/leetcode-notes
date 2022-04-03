# 2226. Maximum Candies Allocated to K Children

## Description
You are given a `0`-indexed integer array `candies`. Each element in the array denotes a pile of candies of size `candies[i]`. You can divide each pile into any number of sub piles, but you cannot merge two piles together.

You are also given an integer `k`. You should allocate piles of candies to `k` children such that each child gets the same number of candies. Each child can take at most one pile of candies and some piles of candies may go unused.

Return the maximum number of candies each child can get.

**Example:**
```
Input: candies = [5,8,6], k = 3
Output: 5
Explanation: We can divide candies[1] into 2 piles of size 5 and 3, and candies[2] into 2 piles of size 5 and 1. We now have five piles of candies of sizes 5, 5, 3, 5, and 1. We can allocate the 3 piles of size 5 to 3 children. It can be proven that each child cannot receive more than 5 
```

## Classic Solution
Suppose we give each child `m` candies, then for each pile of `candies[i]`, we can divide out at most `candies[i] / m` sub piles with each pile `m` candies.

We can sum up all the sub piles we can divide out, then compare with the `k` children.

If `k > sum`,
we don't allocate to every child,
since the pile of m candidies it too big,
so we assign `right = m - 1`.

If `k <= sum`,
we are able to allocate to every child,
since the pile of `m` candidies is small enough
so we assign `left = m`.

We repeatly do this until `left == right`, and that's the maximum number of candies each child can get.

A small tip here is that since we want to find the last valid element, we use `mid = (left + right + 1) / 2`.

```C++
int maximumCandies(vector<int>& candies, long long k) {
    int low = 0;
    int high = 10000000;
    while(low < high) {
        int mid = high - ((high - low) >> 1);
        long long sum = 0;
        for(auto& candy : candies) {
            sum += candy / mid;
        }
        
        if(sum >= k)
            low = mid;
        else
            high = mid - 1;
    }
    return high;
}
```