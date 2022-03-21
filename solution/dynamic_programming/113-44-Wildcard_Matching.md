# 44. Wildcard Matching

## Description
Given an input string (s) and a pattern (p), implement wildcard pattern matching with support for '?' and '*' where:

'?' Matches any single character.
'*' Matches any sequence of characters (including the empty sequence).
The matching should cover the entire input string (not partial).

**Example:**
```
Input: s = "aa", p = "*"
Output: true
Explanation: '*' matches any sequence.
```

## My Solution
This question is a bit easier if you know how to solve [Regular Expression Matching](https://leetcode.com/problems/regular-expression-matching/).


Suppose `dp[i][j]` represents if `s[0..i-1]` matches `p[0..j-1]`. Here's the deduction rule:

- `p[j-1] != '*'`: `dp[i][j] = (s[i-1]==p[j-1] || p[j-1] == '?') && dp[i-1][j-1]` 
- `p[j-1] == '*'`
    Since here `'*'` can match any sequence, so
    - `"*"` matches empty string: `dp[i][j-2]`
    - `"*"` matches `s[i-1]`: `dp[i-1][j]`. Here, we erase the `s[i-1]` and compares `s[0..i-2]` with `p[0..j]` continually. 

For the base case:
- `dp[0][0] = true`
- `dp[i][0] = false`, since emtpy pattern cannot match any string
- `dp[0][j]`: only the proceeding `'*'` matches an empty string.

```C++
bool isMatch(string s, string p) {
    if(p.empty())
        return s.empty();
    
    int m = s.size();
    int n = p.size();
    vector<vector<bool>> dp(m+1, vector<bool>(n+1, false));
    
    dp[0][0] = true;
    for(int j = 1; j <= n; j++) {
        if(p[j-1] != '*')
            break;
        dp[0][j] = true;
    }
    
    for(int i = 1; i <= m; i++) {
        for(int j = 1; j <= n; j++) {
            if(p[j-1] != '*')
                dp[i][j] = (s[i-1] == p[j-1] || p[j-1] == '?') && dp[i-1][j-1];
            else
                dp[i][j] = dp[i][j-1] || dp[i-1][j];
        }
    }
    return dp[m][n];
}
```

## Classic Solution
greedy solution with idea of DFS.

`starj` stores the position of last `'*'` in `p`

`last_match` stores the position of the previous matched char in `s` after a `'*'`

e.g. 
```
s: a c d s c d
p: * c d
```
after the first match of the *, `starj = 0` and `last_match = 1`

when we come to `i = 3` and `j = 3`, we know that the previous match of * is actually wrong, 
(the first branch of DFS we take is wrong)
then it resets `j = starj + 1`

since we already know `i = last_match` will give us the wrong answer

so this time `i = last_match+1` and we try to find a longer match of `'*'`

then after another match we have `starj = 0` and `last_match = 4`, which is the right solution

since we don't know where the right match for `'*'` ends, we need to take a guess (one branch in DFS), 

and store the information(`starj` and `last_match`) so we can always backup to the last correct place and take another guess.

```C++
 bool isMatch(string s, string p) {
        int i = 0, j = 0;
        int m = s.length(), n = p.length();
        int last_match = -1, starj = -1;
        while (i < m){
            if (j < n && (s[i] == p[j] || p[j] == '?')){
                i++; j++;
            }
            else if (j < n && p[j] == '*'){
                starj = j;
                j++;
                last_match = i;
            }
            else if (starj != -1){
                j = starj + 1;
                last_match++;
                i = last_match;
            }
            else return false;
        }
        while (p[j] == '*' && j <n) j++;
        return j == n;
    }
```