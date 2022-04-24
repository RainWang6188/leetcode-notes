# 290. Word Pattern

## Description

Given a `pattern` and a string `s`, find if `s` follows the same `pattern`.

Here follow means a full match, such that there is a bijection between a letter in `pattern` and a non-empty word in `s`.

**Example:**
```
Input: pattern = "abba", s = "dog cat cat dog"
Output: true
```

## Classic Solution

### 1. stringstream >>

Instead of construct a bijection mapping between each character in the `pattern` and each word in the `s`, this method maps both the character and the word to a same integer if they're bijected.

Note that map will assign `0` to non-exist keys, so the mapping integer will counting backwards from `n` to `1`.

```C++
bool wordPattern(string pattern, string s) {
    unordered_map<char, int> p2i;
    unordered_map<string, int> str2i;
    stringstream ss(s);
    int n = pattern.size();
    int index = n;
    for(string word; ss >> word; index--) {
        if(!index || p2i[pattern[n-index]] != str2i[word])
            return false;
        p2i[pattern[n-index]] = index;
        str2i[word] = index;
    }
    return !index;
}
```

### 2. stringstream getline()

This method uses a set to store used words in the string `s`.

```C++
bool wordPattern(string pattern, string s) {
    unordered_map<char, string> p2w;
    unordered_set<string> used;
    string word;
    stringstream ss(s);
    int index = 0;
    while(getline(ss, word, ' ') && index < pattern.size()) {
        auto it = p2w.find(pattern[index]);
        bool wordUsed = used.find(word) != used.end();
        if(it == p2w.end() && wordUsed)
            return false;
        else if(it != p2w.end() && it->second != word)
            return false;
        
        p2w[pattern[index++]] = word;
        used.insert(word);
    }
    return true;
}
```