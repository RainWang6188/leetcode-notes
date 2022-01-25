# 139. Word Break

## Description
Given a string `s` and a dictionary of strings `wordDict`, return `true` if s can be segmented into a space-separated sequence of one or more dictionary words.

**Note** that the same word in the dictionary may be reused multiple times in the segmentation.

**Example:**
```
Input: s = "applepenapple", wordDict = ["apple","pen"]
Output: true
Explanation: Return true because "applepenapple" can be segmented as "apple pen apple".
Note that you are allowed to reuse a dictionary word.
```

## My Solution - Brute Force
My solution is brute force whose time comlexity is $O(2^N)$, which will get a TLE in leetcode.

```C++
bool wordBreak(string s, vector<string>& wordDict) {
    unordered_set<string> dict(wordDict.begin(), wordDict.end());
    
    return search(s, dict);        
}

bool search(string s, unordered_set<string> dict) {
    if(s.size() == 0 || dict.find(s) != dict.end())
        return true;
    
    for(int len = 1; len <= s.size(); len++) {
        if(dict.find(s.substr(0, len)) != dict.end() && search(s.substr(len), dict))
            return true;
    }
    
    return false;
}
```

## Classic Soltuion
Here's the classic dynamic programming solution.

Suppose `dp[i]` represents a boolean value indicates if there's a valid word in the dictionary ends at `i`.

`dp[i]` is true iff `dp[i-len]` is true **AND** the substring `s(i-len+1, i)` is in the dictionary.

In the following code, `i - len == -1` is to make sure the initial character (`s[0]`) will be taken into account.

```C++
bool wordBreak(string s, vector<string>& wordDict) {
    unordered_set<string> dict(wordDict.begin(), wordDict.end());
    
    return wordBreak(s, dict);        
}

bool wordBreak(string s, unordered_set<string>& dict) {
    vector<bool> dp(s.size(), false);
    
    for(int i = 0; i < s.size(); i++) {
        for(int len = 1; i - len >= -1; len++) {
            if((i - len == -1 || dp[i-len]) && dict.find(s.substr(i-len+1, len)) != dict.end()) {
                dp[i] = true;
                break;
            }
        }
    }
    return dp[s.size()-1];
}
```

Also, we can rewrite the code to avoid corner cases check.
```C++
bool wordBreak(string s, unordered_set<string>& dict) {
    vector<bool> dp(s.size() + 1, false);
    dp[0] = true;
    
    for(int i = 1; i <= s.size(); i++) {
        for(int len = 1; i - len >= 0; len++) {
            if(dp[i-len] && dict.find(s.substr(i-len, len)) != dict.end()) {
                dp[i] = true;
                break;
            }
        }
    }
    return dp[s.size()];
}
```