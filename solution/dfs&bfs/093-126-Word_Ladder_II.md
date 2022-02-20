# 126. Word Ladder II
## Description
A transformation sequence from word `beginWord` to word `endWord` using a dictionary `wordList` is a sequence of words `beginWord -> s1 -> s2 -> ... -> sk` such that:

- Every adjacent pair of words differs by a single letter.
- Every `si` for `1 <= i <= k` is in `wordList`. Note that `beginWord` does not need to be in `wordList`.
- `sk == endWord`

Given two words, `beginWord` and `endWord`, and a dictionary `wordList`, return all the **shortest transformation sequences** from `beginWord` to `endWord`, or an empty list if no such sequence exists. Each sequence should be returned as a list of the words `[beginWord, s1, s2, ..., sk]`.

 

**Example 1:**
```
Input: beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log","cog"]
Output: [["hit","hot","dot","dog","cog"],["hit","hot","lot","log","cog"]]
Explanation: There are 2 shortest transformation sequences:
"hit" -> "hot" -> "dot" -> "dog" -> "cog"
"hit" -> "hot" -> "lot" -> "log" -> "cog"
```
## My Solution
### 1. DFS
This DFS method is basically the same with the previous question, but I change the method to find the next valid word.

Since DFS takes exponential time complexity, it will receive a TLE in the OJ.
```C++
class Solution {
public:
    vector<vector<string>> findLadders(string beginWord, string endWord, vector<string>& wordList) {
        unordered_map<string, bool> dict;
        for(auto& word : wordList) {
            dict[word] = false;
        }
        
        if(dict.find(endWord) == dict.end())
            return res;
        
        vector<string> currRes = {};
        
        dfs(beginWord, beginWord, endWord, dict, currRes);
        
        return res;
    }

private:
    vector<vector<string>> res = {};
    
    void dfs(string currWord, string beginWord, string endWord, unordered_map<string, bool>& dict, vector<string>& currRes) {
        currRes.push_back(currWord);
        if(currWord != beginWord || dict.find(beginWord) != dict.end())
            dict.find(currWord)->second = true;
        
        if(currWord == endWord) {
            if(res.size() == 0) {
                res.push_back(currRes);
            }
            else {
                if(currRes.size() < res.front().size()) {
                    res.clear();
                    res.push_back(currRes);
                }
                else if(currRes.size() == res.front().size()) {
                    res.push_back(currRes);
                }
            }
            
            dict.find(endWord)->second = false;
            currRes.pop_back();
            return;
        }
        
        for(int i = 0; i < currWord.size(); i++) {
            char c = currWord[i];
            for(int j = 0; j < 26; j++) {
                currWord[i] = 'a' + j;
                
                unordered_map<string, bool>::iterator it = dict.find(currWord);
                if(it != dict.end() && it->second == false) {
                    dfs(it->first, beginWord, endWord, dict, currRes);
                }
            }
            currWord[i] = c;
        }
        
        if(currWord != beginWord || dict.find(beginWord) != dict.end())
            dict.find(currWord)->second = false;
        currRes.pop_back();
    }
};
```
### 2. BFS

## Classic Solution

