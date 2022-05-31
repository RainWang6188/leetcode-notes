# 1461. Check If a String Contains All Binary Codes of Size k

## Description

Given a binary string `s` and an integer `k`, return `true` if every binary code of length `k` is a substring of `s`. Otherwise, return `false`.

**Example:**
```
Input: s = "00110110", k = 2
Output: true
Explanation: The binary codes of length 2 are "00", "01", "10" and "11". They can be all found as substrings at indices 0, 1, 3 and 2 respectively.
```

## My Solution

Using a set of store all the possible substrings of `s`, and check if it contains the $2^k$ permutations of binary codes.

```C++
bool hasAllCodes(string s, int k) {
    int len = s.size();
    unordered_set<string> substrings;
    
    for(int i = 0; i <= len - k; i++)
        substrings.insert(s.substr(i, k));
    
    return substrings.size() == pow(2, k);
}
```

## Classic Solution

[Here](https://leetcode.com/problems/check-if-a-string-contains-all-binary-codes-of-size-k/solution/)'s another solution using [rolling hash](https://en.wikipedia.org/wiki/Rolling_hash).