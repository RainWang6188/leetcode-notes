# 127. Word Ladder
## Description
A **transformation sequence** from word `beginWord` to word `endWord` using a dictionary `wordList` is a sequence of words `beginWord -> s1 -> s2 -> ... -> sk` such that:

Every adjacent pair of words differs by a single letter.
Every `si` for `1 <= i <= k` is in `wordList`. Note that `beginWord` does not need to be in `wordList`.
sk == `endWord`
Given two words, `beginWord` and `endWord`, and a dictionary `wordList`, return the number of words in the shortest transformation sequence from `beginWord` to `endWord`, or `0` if no such sequence exists.

**Example 1:**
```
Input: beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log","cog"]
Output: 5
Explanation: One shortest transformation sequence is "hit" -> "hot" -> "dot" -> "dog" -> cog", which is 5 words long.
```

## My Solution
My solution is an ordinary DFS, which traverse the words in the `wordList` which differ with the `currWord` by a single letter and update the `count` along the search.

However, it receives TLE in the OJ.
```C++
class Solution {
public:
    int ladderLength(string beginWord, string endWord, vector<string>& wordList) {
        if(beginWord.size() != endWord.size())
            return false;
        vector<bool> visited(wordList.size(), false);
        bool end_in_list = false;
        for(int i = 0; i < wordList.size(); i++) {
            string word = wordList[i];
            visited[i] = (word.size() != beginWord.size());
            if(word == endWord)
                end_in_list = true;
        }
        if(!end_in_list)    return false;
        
        for(int i = 0; i < wordList.size(); i++) {
            int count = 1;
            if(!visited[i] && diff_one_letter(beginWord, wordList[i]))
                dfs(i, endWord, wordList, visited, count);
        }
        return min_trans == INT_MAX ? 0 : min_trans;        
    }

private:
    int min_trans = INT_MAX;
    
    bool diff_one_letter(string str1, string str2) {
        if(str1.size() != str2.size())
            return false;
        int count = 0;
        for(int i = 0; i < str1.size(); i++)
            if(str1[i] != str2[i])
                count++;
        return count == 1;
    }
    
    void dfs(int currIndex, string endWord, vector<string> &wordList, vector<bool> &visited, int& count) {
        string currWord = wordList[currIndex];
        if(currWord == endWord) {
            min_trans = min(count + 1, min_trans);
            return;
        }
        
        visited[currIndex] = true;
        count++;
        for(int i = 0; i < wordList.size(); i++) {
            if(!visited[i] && diff_one_letter(currWord, wordList[i]))
                dfs(i, endWord, wordList, visited, count);
        }
        count--;
        visited[currIndex] = false;
    }
};
```

## Classic Solution
### 1. BFS
This problem has a nice BFS structure.

Suppose any two strings with only one letter difference in the `wordList` are two connected nodes of a graph, then we have an undirected graph the neighbors of a node `str` are all the strings with one letter difference.

The idea is simpy to start from the `beginWord`, then visit its neighbors, then the non-visited neighbors of its neighbors until we arrive at the `endWord`. This is a typical BFS structure.

**Note:** 
1. A key to prevent the TLE is traversing all the possible strings with one letter difference and checking if they're in the list. Since the length of each string is guaranteed to be less than or equal to `10` which is definitely takes less time when traversing the list where there're thousands of strings in the list.
2. Since the erase the pushed string from the list as visited, we should use an `unordered_set` since it supports `set::erase(val)` and takes `O(1)` in average while the parameter of `vector::erase()` should only be an iterator so that finding that iterator requires traversing the whole vector in `O(N)`.

```C++
    int ladderLength(string beginWord, string endWord, vector<string>& wordList) {
        int count = 1;
        unordered_set dict(wordList.begin(), wordList.end());        
        queue<string> q;
        
        q.push(beginWord);
        while(!q.empty()) {
            int currSize = q.size();
            for(int i = 0; i < currSize; i++) {
                string currWord = q.front();
                q.pop();
                if(currWord == endWord)
                    return count;
                
                for(int i = 0; i < currWord.size(); i++) {
                    char c = currWord[i];
                    for(int j = 0; j < 26; j++) {
                        currWord[i] = 'a' + j;
                        if(dict.find(currWord) != dict.end()) {
                            q.push(currWord);
                            dict.erase(currWord);
                        }
                    }
                    currWord[i] = c;
                }
            }
            count++;
        }
        
        return 0;
    }
```
### 2. Two-End BFS
The above code starts from a single end `beginWord`. We may also start from the `endWord` simultaneously. Once we meet the same word, we are done. This [link](https://leetcode.com/problems/word-ladder/discuss/40711/Two-end-BFS-in-Java-31ms.) provides such a two-end search solution. I rewrite the code below for better readability. This solution uses two pointers phead and ptail to switch to the smaller set at each step to save time.

At each iteration, `beginSet` and `endSet` only contains nodes from previous layer (since we don't need to keep all the previous nodes). Since we make sure the size of `beginSet` is no more than the `endSet`, we always insert elements into the `beginSet` and check if this newly inserted element has already in the `endSet`. When this element is inserted, we erase it from the `dict`, which is equivalent to marking it as visited to avoid duplicate access.

**Note:** BFS is not bound to `queue`, sometimes `set` is better since we can store only the current layer in the `set` and it is more convient to check if it already contains some elements.

```C++
    int ladderLength(string beginWord, string endWord, vector<string>& wordList) {
        unordered_set<string> dict(wordList.begin(), wordList.end());
        unordered_set<string> beginSet;
        unordered_set<string> endSet;
        int count = 1;
        
        if(dict.find(endWord) == dict.end())
            return 0;
        
        beginSet.insert(beginWord);
        endSet.insert(endWord);
        
        while(!beginSet.empty() && !endSet.empty()) {
            if(beginSet.size() > endSet.size()) {
                beginSet.swap(endSet);
            }
            
            unordered_set<string> temp;
            for(unordered_set<string>::iterator it = beginSet.begin(); it != beginSet.end(); it++) {
                string str = *it;
                for(int i = 0; i < str.size(); i++) {
                    char c = str[i];
                    for(int j = 0; j < 26; j++) {
                        str[i] = 'a' + j;
                        if(endSet.find(str) != endSet.end())
                            return count + 1;
                        if(dict.find(str) != dict.end()) {
                            temp.insert(str);
                            dict.erase(str);
                        }
                    }
                    str[i] = c;
                }
            }
            count++;
            beginSet.swap(temp);
        }
        return 0;
    }
```