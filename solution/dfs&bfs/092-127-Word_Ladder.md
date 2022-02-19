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
