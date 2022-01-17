# 3. Longest Substring Without Repeating Characters

## Description
Given a string `s`, find the length of the **longest substring** without repeating characters.

**Example:**
```
Input: s = "abcabcbb"
Output: 3
Explanation: The answer is "abc", with the length of 3.
```
## My Solution
My solution is kind of brute force. Traverse the string and when an duplicate character is detected, update the `max_len` and restart traversal from the next character of the previous duplicate element.

```C++
int lengthOfLongestSubstring(string s) {
    int n = s.size();
    int index = 0;
    int max_len = 0;
    int cur_len = 0;
    map<char, int> dict;
    
    while(index < n) {
        if(dict.count(s[index]) != 0) {
            max_len = max(max_len, cur_len);
            index = dict[s[index]] + 1;
            cur_len = 0;
            dict.clear();
        }
        else {
            dict[s[index]] = index;
            index++;
            cur_len++;
        }
    }
    
    return max(max_len, cur_len);
}
```

## Classic Solution - Sliding Window
### 1. Ordinary Sliding Window
Here's an ordinary sliding window solution, where we extend `end` as far as possible until a duplicate character is encountered, then we extract `start` until that duplicate is resolved. Record the maximum length of substring during the slide.

```C++
int lengthOfLongestSubstring(string s) {
    if(s.size() == 0) return 0;
    vector<int> dict(128, 0);
    int max_len = 0;
    
    for(int start = 0, end = 0; end < s.size(); end++) {
        if(++dict[s[end]] > 1) {
            while(s[start] != s[end])
                dict[s[start++]]--;
            dict[s[start++]]--;
        }
        else {
            max_len = max(max_len, end - start + 1);
        }
    }
    return max_len;
}
```
### 2. Optimized Sliding Window

The idea is similar, but it uses two pointers `start` and `end` to mark the current substring. Also, here the value of key in the `dict` is the index of the character, so that we can easily resolve the duplicate condition, and we don't need to clear the dictionary now and then, only update is needed.

Here's the key: `start` only moves forward to make sure we're checking the current window. That's why `start = max(dict[s[end]] + 1, start)` is necessary.

```C++
int lengthOfLongestSubstring(string s) {
    int max_len = 0;
    map<char, int> dict;
    
    for(int start = 0, end = 0; end < s.size(); end++) {
        if(dict.count(s[end]) != 0) {
            start = max(dict[s[end]] + 1, start);
        }
        dict[s[end]] = end;
        max_len = max(max_len, end - start + 1);
    }   
    return max_len;
}
```

The hashmap can be replaced by bitmap for less memory cost.