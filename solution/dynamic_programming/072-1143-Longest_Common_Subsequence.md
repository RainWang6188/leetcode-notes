# 1143. Longest Common Subsequence

## Description
Given two strings `text1` and `text2`, return the length of their longest **common subsequence**. If there is no common subsequence, return `0`.

A **subsequence** of a string is a new string generated from the original string with some characters (can be none) deleted without changing the relative order of the remaining characters.

- For example, "ace" is a subsequence of "abcde".


A **common subsequence** of two strings is a subsequence that is common to both strings.

**Example:**
```
Input: text1 = "abcde", text2 = "ace" 
Output: 3  
Explanation: The longest common subsequence is "ace" and its length is 3.
```

## Classic Solution
Here's a classic dynamic programming solution.

Suppose `dp[i][j]` represents the longest common subsequence of string `text1(0,i)` and string `text2(0,j)`. As a result, the state trasition function can be derived as follows:
$$
dp[i+1][j+1] = 
\begin{cases}
    dp[i][j] + 1 & \text{if } text1[i]==text2[j] \\
    \max \big( dp[i+1][j],\ dp[i][j+1] \big) & \text{otherwise}
\end{cases}
$$

Here's the code.
```C++
int longestCommonSubsequence(string text1, string text2) {
    vector<vector<int>> dp(text1.size() + 1, vector<int>(text2.size() + 1, 0));
    
    for(int i = 0; i < text1.size(); i++) {
        for(int j = 0; j < text2.size(); j++) {
            if(text1[i] == text2[j])
                dp[i+1][j+1] = dp[i][j] + 1;
            else 
                dp[i+1][j+1] = max(dp[i+1][j], dp[i][j+1]);
        }
    }
    return dp[text1.size()][text2.size()];
}
```

### Space Optimization
As you can see, the above method only needs information of previous row to update current row. So we actually only need to maintain a **two-row** 2D array to save and update the matching results.

```C++
int longestCommonSubsequence(string text1, string text2) {
    if(text1.size() < text2.size())
        swap(text1, text2);
    
    vector<vector<int>> dp(2, vector<int>(text2.size() + 1, 0));
    
    for(int i = 0; i < text1.size(); i++) {
        for(int j = 0; j < text2.size(); j++) {
            if(text1[i] == text2[j])
                dp[!(i%2)][j+1] = dp[i%2][j] + 1;
            else
                dp[!(i%2)][j+1] = max(dp[!(i%2)][j], dp[i%2][j+1]);
        }
    }
    return dp[text1.size()%2][text2.size()];
}
```

Actually, it can be further optimized to save half space by using only one-row 1D array and two variables to record matching results.