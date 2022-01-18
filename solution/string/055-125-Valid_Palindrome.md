# 125. Valid Palindrome

## Description
A phrase is a palindrome if, after converting all uppercase letters into lowercase letters and removing all non-alphanumeric characters, it reads the same forward and backward. Alphanumeric characters include **letters** and **numbers**.

Given a string `s`, return true if it is a palindrome, or false otherwise.

**Example:**
```
Input: s = "A man, a plan, a canal: Panama"
Output: true
Explanation: "amanaplanacanalpanama" is a palindrome.
```
## My Solution
This is an intuitive solution in two passes.

In the first pass, we delete all the non-alphanumeric characters and generate the non-alphanumeric string `t`.

In the second pass, we check if `t` is a palindrome.

```C++
bool isPalindrome(string s) {
    string t = "";
    for(auto& c : s)
        if('a' <= c && c <= 'z' ||
            'A' <= c && c <= 'Z' ||
            '0' <= c && c <= '9')
            t += (c >= 'a') ? c : (c >= 'A' ? c - 'A' + 'a' : c);
    
    int n = t.size();
    for(int i = 0; i < n / 2; i++)
        if(t[i] != t[n-i-1])
            return false;
    return true;
}
```
## Classic Solution
Here is an one pass solution using two pointers scanning the string from both ends until collide.

During each iteration, we ommit non-alphanumeric characters at both ends and then examine the equality after converting that two characters to lower case letters.

**Note:**

- Using two pointers to avoid two passes.
- `isalpha()`, `isalnum()`, `islower()`, `isupper()`, `toupper()`, `tolower()` is implemented in C++ already.

```C++
bool isPalindrome(string s) {
    for(int start = 0, end = s.size()-1; start < end; start++, end--) {
        while(!isalnum(s[start]) && start < end) start++;
        while(!isalnum(s[end]) && start < end) end--;
        if(tolower(s[start]) != tolower(s[end]))
            return false;
    }
    return true;
}
```