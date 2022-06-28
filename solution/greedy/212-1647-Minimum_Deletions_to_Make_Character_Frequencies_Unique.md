# 1647. Minimum Deletions to Make Character Freqencies Unique

## Description

A string `s` is called **good** if there are no two different characters in `s` that have the same frequency.

Given a string `s`, return the minimum number of characters you need to delete to make `s` **good**.

The **frequency** of a character in a string is the number of times it appears in the string. For example, in the string `"aab"`, the frequency of `'a'` is `2`, while the frequency of `'b'` is `1`.


**Example:**
```
Input: s = "ceabaacb"
Output: 2
Explanation: You can delete both 'c's resulting in the good string "eabaab".
Note that we only care about characters that are still in the string at the end (i.e. frequency of 0 is ignored).
```

## Classic Solution

We can use an `std::unordered_set` to store the valid frequencies of each character (i.e. after deletion).

`set::insert` returns a `pair<iterator, bool>` pair, with its member pair::first set to an iterator pointing to either the newly inserted element or to the equivalent element already in the set. The pair::second element in the pair is set to true if a new element was inserted or false if an equivalent element already existed.

As a result, we will try to insert the frequency of each character to the `unordered_set`, if its value is already used (in the set), then we delete one character and try insertion repeatedly.

```C++
int minDeletions(string s) {
    vector<int> count(26, 0);
    for(const auto& ch : s)
        count[ch-'a']++;

    int res = 0;
    unordered_set<int> used;
    for(auto& freq : count) {
        while(freq > 0 && !used.insert(freq).second) {
            freq--;
            res++;
        }
    }
    return res;
}
```