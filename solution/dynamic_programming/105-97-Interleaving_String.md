# 97. Interleaving String

## Description
Given strings `s1`, `s2`, and `s3`, find whether `s3` is formed by an interleaving of `s1` and `s2`.

An interleaving of two strings `s` and `t` is a configuration where they are divided into non-empty substrings such that:

- `s = s1 + s2 + ... + sn`
- `t = t1 + t2 + ... + tm`
- `|n - m| <= 1`
- The interleaving is `s1 + t1 + s2 + t2 + s3 + t3 + ...` or `t1 + s1 + t2 + s2 + t3 + s3 + ...`

**Note:** `a + b` is the concatenation of strings `a` and `b`.

**Example:**
```
Input: s1 = "aabcc", s2 = "dbbca", s3 = "aadbbcbcac"
Output: true
```
## My Solution
Here's my DP solution.

Suppose `dp[i][j]` denotes whether `s3[0, i+j-1]` is an interleaving of `s1[0, i-1]` and `s2[0, j-1]`.

In basic case:
- `i == 0 && j == 0`: an empty string `s3` is an interleaving of two empty strings, TRUE.

- `i == 0`: If only `s1` is empty, then if previous `s2` position is interleaving and current `s2` position char is equal to `s3` current position char, it is considered interleaving.

- `j == 0`: similar idea applies to when s2 is empty.

- `i != 0 && j != 0`: when both `s1` and `s2` are not empty, then if we arrive `i`, `j` from `i-1`, `j`, then if `i-1`,`j` is already interleaving and `i` and current `s3` position equal, it's interleaving. If we arrive `i`,`j` from `i`, `j-1`, then if `i`, `j-1` is already interleaving and `j` and current `s3` position equal. it is interleaving.

```C++
    bool isInterleave(string s1, string s2, string s3) {
        if(s3.size() != s1.size() + s2.size())
            return false;
        
        vector<vector<bool>> dp(s1.size() + 1, vector<bool>(s2.size() + 1, false));
        
        for(int i = 0; i <= s1.size(); i++) {
            for(int j = 0; j <= s2.size(); j++) {
                if(!i && !j)
                    dp[i][j] = true;
                else if(!i) {
                    dp[i][j] = (dp[i][j-1] && s2[j-1] == s3[j-1]);
                }
                else if(!j) {
                    dp[i][j] = (dp[i-1][j] && s1[i-1] == s3[i-1]);
                }
                else {
                    dp[i][j] = (dp[i-1][j] && s1[i-1] == s3[i+j-1]) || (dp[i][j-1] && s2[j-1] == s3[i+j-1]);
                }
            }
        }
        return dp[s1.size()][s2.size()];
    }
```
