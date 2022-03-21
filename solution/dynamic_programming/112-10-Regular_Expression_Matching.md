# 10. Regular Expression Matching

## Description
Given an input string `s` and a pattern `p`, implement regular expression matching with support for '`.'` and `'*'` where:

- `'.'` Matches any single character.​​​​
- `'*'` Matches zero or more of the preceding element.

The matching should cover the **entire** input string (not partial).

**Example:**
```
Input: s = "aa", p = "a*"
Output: true
Explanation: '*' means zero or more of the preceding element, 'a'. Therefore, by repeating 'a' once, it becomes "aa".
```

## Classic Solution
Suppose `dp[i][j]` represents if `s[0..i-1]` matches `p[0..j-1]`. Here's the deduction rule:

- `p[j-1] != '*'`: `dp[i][j] = (s[i-1]==p[j-1] || p[j-1] == '.') && dp[i-1][j-1]` 
- `p[j-1] == '*'`
    we denote `p[j-2]` as `X`, then `dp[i][j]` is determined by the following two cases
    
    - `"X*"` repeats 0 time and matches empty string: `dp[i][j-2]`
    - `"X*"` repeats at least 1 time: `(s[i-1]==p[j-2] || p[j-2]=='.') && dp[i-1][j]`. Here, we erase the `s[i-1]` if it matches and compares `s[0..i-2]` with `p[0..j]` continually. 


```C++
bool isMatch(string s, string p) {
    if(p.empty() || p[0] == '*')
        return false;
    
    int m = s.size();
    int n = p.size();
    vector<vector<bool>> dp(m+1, vector<bool>(n+1, false));
    
    dp[0][0] = true;
    for(int j = 1; j < n; j+=2)
        if(p[j] == '*' && dp[0][j-1])
            dp[0][j+1] = true;
    
    for(int i = 1; i <= m; i++) {
        for(int j = 1; j <= n; j++) {
            if(p[j-1] != '*') 
                dp[i][j] = (s[i-1] == p[j-1] || p[j-1] == '.') && dp[i-1][j-1];
            else {
                dp[i][j] = dp[i][j-2] || (s[i-1] == p[j-2] || p[j-2] == '.') && dp[i-1][j];
            }
        }
    }
    return dp[m][n];
}
```