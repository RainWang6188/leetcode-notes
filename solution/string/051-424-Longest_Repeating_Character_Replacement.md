# 424. Longest Repeating Character Replacement
## Description
You are given a string `s` and an integer `k`. You can choose any character of the string and change it to any other uppercase English character. You can perform this operation at most `k` times.

Return the length of the longest substring containing the same letter you can get after performing the above operations.

**Example:**
```
Input: s = "AABABBA", k = 1
Output: 4
Explanation: Replace the one 'A' in the middle with 'B' and form "AABBBBA".
The substring "BBBB" has the longest repeating letters, which is 4.
```
## Classic Solution
This problem can be reiterated as finding the longest substring with less than `k` replacement (i.e. `longest substring where (length - max occurrence) <= k`).

Since we are only interested in the longest valid substring, our sliding windows need not shrink, even if a window may cover an invalid substring. We either grow the window by appending one char on the right, or shift the whole window to the right by one. The reason for that is the current window is invalid, any index smaller than original `start`, will never have the chance to lead a longer valid substring than current length of our window. 

And we only grow the window when the count of the new char exceeds the historical max count (from a previous window that covers a valid substring).

As a result, we don't care about the max count of the current window, we only care about if the max count exceeds the historical max count. In the code, `max_count` indicates the maximum number of occurance of a single character found **so far** (`s[0] -> s[end]`)


```C++
int characterReplacement(string s, int k) {
    int n = s.size();
    vector<int> count(26, 0);
    int max_count = 0;
    int max_len = 0;

    for(int start = 0, end = 0; end < n; end++) {
        max_count = max(max_count, ++count[s[end]-'A']);
        if(end - start + 1 - max_count > k) {
            count[s[start]-'A']--;
            start++;
        }
        max_len = max(max_len, end-start+1);
    }
    return max_len;
}
```
