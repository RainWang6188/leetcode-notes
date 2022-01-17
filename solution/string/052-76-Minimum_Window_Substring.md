# 76. Minimum Window Substring

## Description
Given two strings `s` and `t` of lengths `m` and `n` respectively, return the **minimum window substring** of `s` such that every character in `t` (**including duplicates**) is included in the window. If there is no such substring, return the empty string `""`.

The testcases will be generated such that the answer is **unique**.

A **substring** is a contiguous sequence of characters within the string.

**Example:**
```
Input: s = "ADOBECODEBANC", t = "ABC"
Output: "BANC"
Explanation: The minimum window substring "BANC" includes 'A', 'B', and 'C' from string t.
```

## Classic Solution - Sliding Window
The question asks us to return the minimum window from the string $S$ which has all the characters of the string $T$. Let us call a window desirable if it has all the characters from $T$.

We can use a simple sliding window approach to solve this problem.

In any sliding window based problem we have two pointers. One `end` pointer whose job is to expand the current window and then we have the `start` pointer whose job is to contract a given window. At any point in time only one of these pointers move and the other one remains fixed.

The solution is pretty intuitive. We keep expanding the window by moving the right pointer. When the window has all the desired characters, we contract (if possible) and save the smallest window till now.

The answer is the smallest desirable window.

**Algorithm**

1. We start with two pointers, `start` and `end` initially pointing to the first element of the string $S$.

2. We use the `end` pointer to expand the window until we get a desirable window i.e. a window that contains all of the characters of $T$.

3. Once we have a window with all the characters, we can move the left pointer ahead one by one. If the window is still a desirable one we keep on updating the minimum window size.

4. If the window is not desirable any more, we repeat $step\ 2$ onwards.

```C++
string minWindow(string s, string t) {
    if(s.size() == 0 || t.size() == 0)
        return "";
    
    vector<int> remain(128, 0);
    int min_len = INT_MAX;
    int left = 0;
    int start = 0, end = 0;
    int required = t.size();
    
    for(int i = 0; i < required; i++)
        remain[t[i]]++;
    
    while(end <= s.size()) {
        if(required) {
            if(end == s.size())
                break;
            remain[s[end]]--;
            if(remain[s[end]] >= 0)
                required--;
            end++;
        }
        else {
            if(end - start < min_len) {
                min_len = end - start;
                left = start;
            }
            remain[s[start]]++;
            if(remain[s[start]] > 0)
                required++;
            start++;
        }
    }
    return min_len == INT_MAX ? "" : s.substr(left, min_len);        
}
```