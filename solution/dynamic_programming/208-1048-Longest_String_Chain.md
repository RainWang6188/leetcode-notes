# 1048. Longest String Chain

## Description
You are given an array of `words` where each word consists of lowercase English letters.

`wordA` is a **predecessor** of `wordB` if and only if we can insert exactly one letter anywhere in `wordA` without changing the order of the other characters to make it equal to `wordB`.

For example, `"abc"` is a predecessor of `"abac"`, while `"cba"` is not a predecessor of `"bcad"`.

A word chain is a sequence of words `[word1, word2, ..., wordk]` with `k >= 1`, where `word1` is a predecessor of `word2`, `word2` is a predecessor of `word3`, and so on. A single word is trivially a word chain with `k == 1`.

Return the length of the longest possible word chain with words chosen from the given list of words.

**Example:**
```
Input: words = ["a","b","ba","bca","bda","bdca"]
Output: 4
Explanation: One of the longest word chains is ["a","ba","bda","bdca"].
```


## Classic Solution

- Deleting a character from a word to find its predecessor is more efficient than trying to insert every possible character into a word to find its successor.

- Using dynamic programming to solve this problem. Suppose `pre` is the predecessor of `word`, and `dp[str]` denotes the longest string chain of the word `str`, then we have the state transition function as follows: `dp[word] = max(dp[word], dp[pre] + 1)`.

```C++
int longestStrChain(vector<string>& words) {
    auto cmp = [](const string& word1, const string& word2) {
        return word1.size() < word2.size();
    };
    sort(words.begin(), words.end(), cmp);
    unordered_map<string, int> dp;
    int maxLen = 0;
    
    for(const auto& word : words) {
        for(int i = 0; i < word.size(); i++) {
            string pre = word.substr(0, i) + word.substr(i + 1);
            dp[word] = max(dp[word], dp.find(pre) == dp.end() ? 1 : dp[pre] + 1);
            maxLen = max(maxLen, dp[word]);
        }
    }
    return maxLen;
}
```