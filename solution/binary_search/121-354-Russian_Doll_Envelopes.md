# 354. Russian Doll Envelopes

## Description
You are given a 2D array of integers `envelopes` where `envelopes[i] = [wi, hi]` represents the width and the height of an envelope.

One envelope can fit into another if and only if both the width and height of one envelope are greater than the other envelope's width and height.

Return the maximum number of envelopes you can Russian doll (i.e., put one inside the other).

Note: You cannot rotate an envelope.

**Example:**
```
Input: envelopes = [[5,4],[6,4],[6,7],[2,3]]
Output: 3
Explanation: The maximum number of envelopes you can Russian doll is 3 ([2,3] => [5,4] => [6,7]).
```

## Classic Solution

The basic algorithm is as follows:
1. Sort the array. Ascend on width and descend on height if width are same.
2. Find the longest increasing subsequence based on height.

This problem is asking for LIS in two dimensions, width and height. Sorting the width reduces the problem by one dimension. If width is strictly increasing, the problem is equivalent to finding LIS in only the height dimension. However, when there is a tie in width, a strictly increasing sequence in height may not be a correct solution. For example, `[3,3]` cannot fit in `[3,4]`. Sorting height in descending order when there is a tie prevents such a sequence to be included in the solution.

```C++
int maxEnvelopes(vector<vector<int>>& envelopes) {
    auto cmp = [](vector<int>& env1, vector<int>& env2) {
        if(env1[0] != env2[0])
            return env1[0] < env2[0];
        else
            return env1[1] > env2[1];
    };
    
    sort(envelopes.begin(), envelopes.end(), cmp);
    
    vector<int> dp(envelopes.size(), INT_MAX);
    int maxLen = 0;
    for(auto& item : envelopes) {
        int currHeight = item[1];
        int low = 0;
        int high = maxLen;
        while(low < high) {
            int mid = low + ((high - low) >> 1);
            if(dp[mid] < currHeight)
                low = mid + 1;
            else
                high = mid;
        }
        dp[high] = currHeight;
        if(high == maxLen)
            maxLen++;
    }
    
    return maxLen;
}
```
