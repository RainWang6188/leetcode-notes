# 5. Longest Palindromic Substring

## Description
Given a string `s`, return the longest palindromic substring in `s`.

**Example:**
```
Input: s = "babad"
Output: "bab"
Explanation: "aba" is also a valid answer.
```

## My Solution
This is sort of brute force. For each possible index, we try to expand as palindrome and update the `max_len` at the same time.

Since a palidrome my have odd or even number of characters, we need to expand in two different ways.

```C++
string longestPalindrome(string s) {
    int n = s.size();
    if(n < 2)
        return s;
    
    int left;
    int max_len = 0;
    for(int i = 0; i < n - 1; i++) {
        int start = i, end = i;
        while(start >= 0 && end < n && s[start] == s[end]) {
            if(end - start + 1 > max_len) {
                max_len = end - start + 1;
                left = start;
            }
            start--;
            end++;
        }
        
        start = i, end = i + 1;
        while(start >= 0 && end < n && s[start] == s[end]) {
            if(end - start + 1 > max_len) {
                max_len = end - start + 1;
                left = start;
            }
            start--;
            end++;     
        }
    }
    return s.substr(left, max_len);
}
```

## Classic Solution
### 1. Optimized Brute Force
In this solution, we skip duplicate characters to avoid checking the parity of the palidrome.

```C++
string longestPalindrome(string s) {
    int n = s.size();
    if(n < 2)
        return s;
    
    int max_len = 0;
    int left = 0;
    
    int mid = 0;
    while(mid < n) {
        int start = mid;
        int end = mid;
        
        while(end < n - 1 && s[end] == s[end+1])
            end++;
        mid = end + 1;
        
        while(start > 0 && end < n - 1 && s[start-1] == s[end+1]) {
            start--;
            end++;
        }
        
        if(end - start + 1 > max_len) {
            max_len = end - start + 1;
            left = start;
        }
    }
    return s.substr(left, max_len);
}
```

### 2. Dynamic Programming 
This problem can be solved by breaking down into subproblems which are reused several times. Overlapping subproblems lead us to Dynamic Programming.
