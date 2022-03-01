# 72. Edit Distance

## Description
Given two strings `word1` and `word2`, return the minimum number of operations required to convert `word1` to `word2`.

You have the following three operations permitted on a word:

- Insert a character
- Delete a character
- Replace a character

**Example:**
```
Input: word1 = "intention", word2 = "execution"
Output: 5
Explanation: 
intention -> inention (remove 't')
inention -> enention (replace 'i' with 'e')
enention -> exention (replace 'n' with 'x')
exention -> exection (replace 'n' with 'c')
exection -> execution (insert 'u')
```
## Classic Solution
Suppose `dp[i][j]` is the minimum number of operations to convert the first `i` characters of `word1` to the first `j` characters of `word2`.

For the base case, to convert a string to an empty string, what we need to do is simply deletion. So `dp[i][0] = i` and `dp[0][j] = j`.

Following is the how to compute `dp[i][j]` (i.e. convert `word1[0, i-1]` to `word2[0, j-1]`) with the prior knowledge of `dp[x][y]` where `x<i && y<j`.

- If `word1[i-1] == word2[j-1]`, then we don't do any operation, `dp[i][j] = dp[i-1][j-1]`

- Otherwise
    - Insert `word2[j-1]` to `word1[0,i-1]`, then we have `dp[i][j] = dp[i][j-1]+1`
    - Delete `word1[i-1]`, then we have `dp[i][j] = dp[i-1][j]+1`
    - Replace `word1[i-1]` with `word2[j-1]`, then we have `dp[i][j] = dp[i-1][j-1]+1`

```C++
    int minDistance(string word1, string word2) {
        int m = word1.size();
        int n = word2.size();
        vector<vector<int>> dp(m+1, vector<int>(n+1, 0));
        
        // dp[i][0] = i
        for(int i = 1; i <= m; i++)
            dp[i][0] = i;
        //dp[0][j] = j
        for(int j = 1; j <= n; j++)
            dp[0][j] = j;
        
        // word1[i-1] == word2[j-1]: dp[i][j] = dp[i-1][j-1] + 1;
        // otherwise:
        //      1) insert:  dp[i][j] = dp[i][j-1] + 1 
        //      2) delete:  dp[i][j] = dp[i-1][j] + 1
        //      3) replace: dp[i][j] = dp[i-1][j-1] + 1
        
        for(int i = 1; i <= m; i++) {
            for(int j = 1; j <= n; j++) {
                if(word1[i-1] == word2[j-1]) {
                    dp[i][j] = dp[i-1][j-1];
                }
                else {
                    dp[i][j] = min({dp[i][j-1], dp[i-1][j], dp[i-1][j-1]}) + 1;
                }
            }
        }
        return dp.back().back();
    }
```

**Optimization:**
Since `dp[i][j]` only depends on `dp[i-1][j-1]`, `dp[i-1][j]` and `dp[i][j-1]`, we simply need a 1D vector instead of the whole 2D vector.

Note that `dp[i-1][j-1]` is in the previous row. When we use 1D vector `dp[]`, updating the current row will erase the previous `dp[i-1]`, so we need to store it before update and store it in an variable `prev_up_left`.

Here's the code:

```C++
    int minDistance(string word1, string word2) {
        int m = word1.size();
        int n = word2.size();
        
        vector<int> dp(n+1, 0);
        for(int j = 1; j <= n; j++)
            dp[j] = j;
        
        for(int i = 1; i <= m; i++) {
            int prev_up_left = dp[0];
            dp[0] = i;
            for(int j = 1; j <= n; j++) {
                int temp = dp[j];
                if(word1[i-1] == word2[j-1])
                    dp[j] = prev_up_left;
                else
                    dp[j] = min({dp[j-1], dp[j], prev_up_left}) + 1;
                
                prev_up_left = temp;
            }
        }
        return dp.back();
    }
```