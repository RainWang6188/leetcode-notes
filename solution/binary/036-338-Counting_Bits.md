# 338. Counting Bits

## Description
Given an integer `n`, return an array `ans` of length `n + 1` such that for each `i (0 <= i <= n)`, `ans[i]` is the number of 1's in the binary representation of `i`.

**Example:**
```
Input: n = 5
Output: [0,1,1,2,1,2]
Explanation:
0 --> 0
1 --> 1
2 --> 10
3 --> 11
4 --> 100
5 --> 101
```
## My Solution
My solution is naive brute force. I compute the number of `1` bits in every integer `i` where `0 <= i <= n` iteratively. The time complexity is $O(N\log N)$
```C++
vector<int> countBits(int n) {
    vector<int> res;
    
    for(int i = 0; i <= n; i++) {
        int count = 0;
        int val = i;
        while(val) {
            val &= (val - 1);
            count++;
        }
        res.push_back(count);
    }
    return res;
}
```

## Classic Solution
### 1. Dynamic Programming 1
I thought about using dynamic programming to solve this question, so that we can use the previous information and avoid replication computation.

```
Index : 0 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15

num :   0 1 1 2 1 2 2 3 1 2 2  3  2  3  3  4
```
Obviously, this is overlap sub problem, and we can come up the DP solution. For now, we need find the function to implement DP.

dp[0] = 0;

dp[1] = dp[0] + 1;

dp[2] = dp[0] + 1;

dp[3] = dp[1] +1;

dp[4] = dp[0] + 1;

dp[5] = dp[1] + 1;

dp[6] = dp[2] + 1;

dp[7] = dp[3] + 1;

dp[8] = dp[0] + 1;

...

This is the function we get, now we need find the other pattern for the function to get the general function. After we analyze the above function, we can get:

dp[0] = 0;

dp[1] = dp[1-1] + 1;

dp[2] = dp[2-2] + 1;

dp[3] = dp[3-2] +1;

dp[4] = dp[4-4] + 1;

dp[5] = dp[5-4] + 1;

dp[6] = dp[6-4] + 1;

dp[7] = dp[7-4] + 1;

dp[8] = dp[8-8] + 1;

...

Obviously, we can find the pattern for above example, so now we get the general function

`dp[index] = dp[index - offset] + 1;`

Here, another very tricky way to compute `index - offset` is `i & (i - 1)`, this will clear the lowest set bit, which is just what we want.

Here's the C++ code:
```C++
vector<int> countBits(int num) {
    vector<int> ret(num+1, 0);
    for (int i = 1; i <= num; ++i)
        ret[i] = ret[i&(i-1)] + 1;
    return ret;
}
```