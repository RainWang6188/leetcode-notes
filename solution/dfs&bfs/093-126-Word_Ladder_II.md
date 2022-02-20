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
## My Solution - DFS

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

## Classic Solution
### 1. BFS + DFS
The basic idea is:

1). Use BFS to find the shortest distance between start and end, tracing the distance of crossing nodes from start node to end node, and store node's next level neighbors to HashMap;

2). Use DFS to output paths with the same distance as the shortest distance from distance HashMap: compare if the distance of the next level node equals the distance of the current node + 1.

```C++
class Solution {
public:
    vector<vector<string>> findLadders(string beginWord, string endWord, vector<string>& wordList) {
        unordered_set<string> wordSet(wordList.begin(), wordList.end()); 
        wordSet.insert(beginWord);
        if(wordSet.find(endWord) == wordSet.end())
            return res;

        bfs(beginWord, endWord, wordSet);
        
        vector<string> currRes = {};
        dfs(beginWord, endWord, currRes);
        
        return res;
    }
    
private:
    vector<vector<string>> res = {};
    
    unordered_map<string, vector<string>> neighbors;
    
    unordered_map<string, int> distance;
    
    void bfs(string beginWord, string endWord, unordered_set<string>& wordSet) {
        distance[beginWord] = 1;
        setNeighbors(wordSet);
        
        queue<string> q;
        q.push(beginWord);
        while(!q.empty()) {
            int currSize = q.size();
            
            for(int i = 0; i < currSize; i++) {
                string currWord = q.front();
                q.pop();

                for(auto& next : neighbors[currWord]) {
                    if(distance.find(next) == distance.end()) {
                        q.push(next);
                        distance[next] = distance[currWord] + 1;
                        if(next == endWord)
                            return;
                    }
                }
            }
        }
    }
    
    void dfs(string currWord, string endWord, vector<string>& currRes) {
        currRes.push_back(currWord);
        if(currWord == endWord) {
            res.push_back(currRes);
            currRes.pop_back();
            return;
        }
        
        for(auto& nextWord : neighbors[currWord]) {
            if(distance[nextWord] == distance[currWord] + 1) {
                dfs(nextWord, endWord, currRes);
            }
        }
        currRes.pop_back();
    }
    
    void setNeighbors(unordered_set<string>& wordSet) {
        for(auto currWord : wordSet) {
            vector<string> currNeighbor = {};
            string oldWord = currWord;
            for(int i = 0; i < currWord.size(); i++) {
                char old_ch = currWord[i];
                for(int j = 0; j < 26; j++) {
                    currWord[i] = 'a' + j;
                    if(currWord != oldWord && wordSet.find(currWord) != wordSet.end()) {
                        currNeighbor.push_back(currWord);
                    }
                }
                currWord[i] = old_ch;
            }
            neighbors[currWord] = currNeighbor; 
        }
    }
};
```
### 2. BFS
This is a standard BFS solution. The only change here is the element in the `queue` is all the possible paths rather than strings.

At each level traversal, we record the visited nodes and remove it from the `wordSet` after each loop. It is essential to remove the visited nodes at the end of each level traversal for the `endWord` may be accessed more than one time when there're several shortest paths. 


```C++
    vector<vector<string>> findLadders(string beginWord, string endWord, vector<string>& wordList) {
        vector<vector<string>> res = {};
        unordered_set<string> wordSet(wordList.begin(), wordList.end()); 
        if(wordSet.find(endWord) == wordSet.end())
            return res;
        
        unordered_set<string> visited;
        queue<vector<string>> q;
        q.push({beginWord});
        visited.insert(beginWord);
        
        while(!q.empty()) {
            int currSize = q.size();
            while(currSize--) {
                vector<string> currPath = q.front();
                q.pop();
                string last = currPath.back();
                
                for(int i = 0; i < last.size(); i++) {
                    char old_ch = last[i];
                    for(char c = 'a'; c <= 'z'; c++) {
                        last[i] = c;
                        if(wordSet.find(last) != wordSet.end()) {
                            vector<string> newPath = currPath;
                            newPath.push_back(last);
                            visited.insert(last);
                            
                            if(last == endWord)
                                res.push_back(newPath);
                            else
                                q.push(newPath);
                        }
                    }
                    last[i] = old_ch;
                }
            }
            for(auto& word : visited)
                wordSet.erase(word);
            visited.clear();
        }
        return res;
    }
```