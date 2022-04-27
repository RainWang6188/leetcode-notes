# 1202. Smallest String with Swaps

## Description

You are given a string `s`, and an array of pairs of indices in the string `pairs` where `pairs[i] = [a, b]` indicates 2 indices(0-indexed) of the string.

You can swap the characters at any pair of indices in the given pairs **any number of times**.

Return the lexicographically smallest string that `s` can be changed to after using the swaps.

**Example:**
```
Input: s = "dcab", pairs = [[0,3],[1,2]]
Output: "bacd"
Explaination: 
Swap s[0] and s[3], s = "bcad"
Swap s[1] and s[2], s = "bacd"
```

## Classic Solution

![demo](https://assets.leetcode.com/users/votrubac/image_1569195129.png)

We can group index pairs which can be swapped using union-find.

And put the characters in lexicographically ascending order for those pairs.

```C++
class Solution {
private:
    int find(vector<int>& ds, int index) {
        return ds[index] < 0 ? index : ds[index] = find(ds, ds[index]);
    }
public:
    string smallestStringWithSwaps(string s, vector<vector<int>>& pairs) {
        vector<int> ds(s.size(), -1);
        vector<vector<int>> groups(s.size());
        for(auto& pair : pairs) {
            int root1 = find(ds, pair[0]);
            int root2 = find(ds, pair[1]);
            if(root1 != root2)
                ds[root2] = root1;
        }
        
        for(int i = 0; i < s.size(); i++)
            groups[find(ds, i)].push_back(i);
        
        for(auto& group : groups) {
            string ss = "";
            for(auto& index : group)
                ss += s[index];
            sort(ss.begin(), ss.end());
            for(int i = 0; i < ss.size(); i++)
                s[group[i]] = ss[i];
        }
        return s;
    }
};
```