# 91. Decode Ways

## Description
A message containing letters from `A-Z` can be **encoded** into numbers using the following mapping:
```
'A' -> "1"
'B' -> "2"
...
'Z' -> "26"
```
To **decode** an encoded message, all the digits must be grouped then mapped back into letters using the reverse of the mapping above (there may be multiple ways). For example, `"11106"` can be mapped into:

- "AAJF" with the grouping `(1 1 10 6)`
- "KJF" with the grouping `(11 10 6)`

Note that the grouping `(1 11 06)` is invalid because `"06"` cannot be mapped into 'F' since "6" is different from `"06"`.

Given a string s containing only digits, return the number of ways to **decode** it.

The test cases are generated so that the answer fits in a 32-bit integer.

**Example:**
```
Input: s = "226"
Output: 3
Explanation: "226" could be decoded as "BZ" (2 26), "VF" (22 6), or "BBF" (2 2 6).
```
## My Solution
For any character `s[i]`, it has two options

1. Decode `s[i]` as a single value (no grouping with others), if `s[i] != 0`
2. Decode `s[i-1, i]` as a value, if the integer value of `s[i-1, i]` is in range of $[10, 26]$.

As a result, the state transition function is as follows:

- if `s[i] != 0`, `dp[i] += dp[i-1]`
- if `s[i-1, i] in [10,26]`, `dp[i] += dp[i-2]`

```C++
int numDecodings(string s) {
    vector<int> dp(s.size(), 0);
    
    for(int i = 0; i < s.size(); i++) {
        if(s[i] >= '1' && s[i] <= '9')
            dp[i] = (!i ? 1 : dp[i-1]);
         
        if(i > 0 && (stoi(s.substr(i-1, 2)) >= 10 && stoi(s.substr(i-1, 2)) <= 26))
            dp[i] += (i == 1 ? 1 : dp[i-2]);
    }
    return dp[s.size()-1];
}
```

**Optimization**
We don't need to whole array, which can be replaced by two variables.

```C++
int numDecodings(string s) {
    int mostPrev = 1;
    int nextPrev = 1;
    
    for(int i = 0; i < s.size(); i++) {
        int curr = 0;
        
        if(s[i] >= '1' && s[i] <= '9')
            curr += mostPrev;
        
        if(i > 0 && (stoi(s.substr(i-1, 2)) >= 10 && stoi(s.substr(i-1, 2)) <= 26))
            curr += nextPrev;
        
        nextPrev = mostPrev;
        mostPrev = curr;
    }
    return mostPrev;
}
```