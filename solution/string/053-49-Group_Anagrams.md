# 49. Group Anagrams
## Description
Given an array of strings `strs`, group the **anagrams** together. You can return the answer in any order.

An **Anagram** is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

**Example:**
```
Input: strs = ["eat","tea","tan","ate","nat","bat"]
Output: [["bat"],["nat","tan"],["ate","eat","tea"]]
```

## My Solution
The idea is intuitive. However, I used a hash map to map the string into index and compare between the vectors takes time. So it received a TLE error in leetcode OJ.
```C++
vector<vector<string>> groupAnagrams(vector<string>& strs) {
    int n = strs.size();
    vector<vector<int>> dict(n, vector<int>(26, 0));
    map<string, int> index;
    vector<vector<string>> res;
    
    for(int i = 0; i < n; i++) {
        for(int j = 0; j < strs[i].size(); j++)
            dict[i][strs[i][j]-'a']++;
        index[strs[i]] = i;
    }
        
    for(int i = 0; i < n; i++) {
        int j = 0;
        for(j = 0; j < res.size(); j++) {
            if(dict[index[res[j][0]]] == dict[i]) {
                res[j].push_back(strs[i]);
                break;
            }
        }
        if(j == res.size())
            res.push_back({strs[i]});
    }
    return res;
}
```
## Classic Solution
A classic solution uses `map<string, vector<string>>`, where the key is the identifier obtained from the alphabet table of string.

Note that the key cannot simply derived from `s += to_string(count[i])` since the number of alphabet used may be more than one digit.

```C++
vector<vector<string>> groupAnagrams(vector<string>& strs) {
    map<string, vector<string>> dict;
    for(auto& str: strs) {
        vector<int> count(26, 0);
        for(int i = 0; i < str.size(); i++)
            count[str[i]-'a']++;
        
        string s = "";
        for(int i = 0; i < 26; i++)
            s += string(count[i], 'a' + i); 
        
        dict[s].push_back(str);
    }
    
    vector<vector<string>> res;
    for(auto& group : dict)
        res.push_back(group.second);
    return res;
}
```
