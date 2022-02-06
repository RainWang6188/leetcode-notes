# 647. Palindromic Substrings

## Description
Given a string `s`, return the number of **palindromic substrings** in it.

A string is a **palindrome** when it reads the same backward as forward.

A **substring** is a contiguous sequence of characters within the string.

**Example:**
```
Input: s = "aaa"
Output: 6
Explanation: Six palindromic strings: "a", "a", "a", "aa", "aa", "aaa".
```

## My Solution
This is a brute force solution. We start from each possible index and try expanding as a palindrome, updating the `count` at the same time.

```C++
int countSubstrings(string s) {
    int n = s.size();
    if(n < 2)
        return 1;
    
    int count = 0;
    for(int mid = 0; mid < s.size(); mid++) {
        int start = mid, end = mid;
        while(start >= 0 && end < n && s[start] == s[end]) {
            count++;
            start--;
            end++;
        }
        
        start = mid, end = mid + 1;
        while(start >= 0 && end < n && s[start] == s[end]) {
            count++;
            start--;
            end++;
        }
    }
    return count;
}
```

## Classic Solution
This problem can be solved using dynamic programming. Some solutions can be viewed [here](https://leetcode.com/problems/palindromic-substrings/discuss/475745/C%2B%2B-dp-solution%3A-recursive-greater-memoization-greater-tabulation).

Basically, it is the same as the [Longest Palindromic Substring](https://leetcode.com/problems/longest-palindromic-substring/) question, but once the `dp[i][j]` is `true`, we increment the `count`.

This dp solution takes longer than my original solution.

```C++
int countSubstrings(string s) {
    int n = s.size();
    if(n < 2)
        return 1;
    
    int count = 0;
    vector<vector<bool>> dp(n, vector<bool>(n, false));
    
    for(int i = n-1; i >= 0; i--)
        for(int j = i; j < n; j++) {
            dp[i][j] = (s[i] == s[j]) && (j - i < 3 || dp[i+1][j-1]);
            if(dp[i][j])
                count++;
        }
    return count;
}
```