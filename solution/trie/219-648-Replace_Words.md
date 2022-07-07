# 648. Replace Words

## Description

In English, we have a concept called **root**, which can be followed by some other word to form another longer word - let's call this word **successor**. For example, when the root `"an"` is followed by the successor word `"other"`, we can form a new word `"another"`.

Given a dictionary consisting of many roots and a sentence consisting of words separated by spaces, replace all the successors in the sentence with the root forming it. If a successor can be replaced by more than one root, replace it with the root that has the shortest length.

Return the `sentence` after the replacement.


**Example 1:**
```
Input: dictionary = ["cat","bat","rat"], sentence = "the cattle was rattled by the battery"
Output: "the cat was rat by the bat"
```

## My Solution

We can use a trie to store all the roots in the dictionary, and search for the shortest prefix of each word of  `sentence`.

```C++
class Trie {
public:
    Trie() {}

    void insert(vector<string>& dict) {
        for(const auto& word : dict) {
            Trie* curr = this;
            for(const auto& ch : word) {
                int index = ch - 'a';
                if(!curr->next[index])
                    curr->next[index] = new Trie();
                curr = curr->next[index];
            }
            curr->isWord = true;
        }
    }

    string searchPrefix(const string& word) {
        string res;
        Trie* curr = this;
        for(const auto& ch : word) {
            int index = ch - 'a';
            if(!curr->next[index])
                return word;
            
            res += ch;
            curr = curr->next[index];
            if(curr->isWord)
                return res;
        }

        return word;
    }

private:
    array<Trie*, 26> next = {};
    bool isWord = false;
};

class Solution {
public:
    string replaceWords(vector<string>& dictionary, string sentence) {
        Trie* trie = new Trie();
        trie->insert(dictionary);

        string res;
        stringstream ss(sentence);
        string word;
        while(getline(ss, word, ' ')) {
            if(!res.empty())
                res += ' ';
            
            res += trie->searchPrefix(word);
        }

        return res;
    }
};
```