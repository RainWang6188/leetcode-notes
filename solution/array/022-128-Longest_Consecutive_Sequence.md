# 128. Longest Consecutive Sequence

## Description
Given an unsorted array of integers `nums`, return the length of the longest consecutive elements sequence.

You must write an algorithm that runs in `O(n)` time.

## My Solution
My solution is acutally not strictly bound to `O(n)` time. I uses a vector of sets to store all the possible consecutive sequence, which is not guaranteed to be consecutive since an element will be put into a set if the difference between its value and the first element of the set is not greater than `nums.size()`. Due to the consecutiveness is not guaranteed, we should check each set and calculate the longest consecutive sequence afterwards. The code is as follows:

```C++
int longestConsecutive(vector<int>& nums) {
    int n = nums.size();
    vector<set<int>> dict;
    
    for(int num : nums) {
        bool inserted = false;
        for(int i = 0; i < dict.size(); i++) {
            if(abs(num - *dict[i].begin()) <= n) {
                dict[i].insert(num);
                inserted = true;
                break;
            }
        }
        if(!inserted) {
            set<int> new_vec = {num};
            dict.push_back(new_vec);
        }
    }
    
    int max_len = 0;
    for(auto single_dict : dict) {
        int count = 1;
        auto it_latter = single_dict.begin();
        it_latter++;

        for(auto it = single_dict.begin(); it_latter != single_dict.end(); it++, it_latter++) {
            if(*it + 1 == *it_latter) {
                count++;
            }
            else {
                max_len = max<int>(max_len, count);
                count = 1;
            }
        }
        max_len = max<int>(max_len, count);
    }
    return max_len;
}
```
## Classic Solution
Using `unordered_set` satisfies the time limit, since each operation of `unordered_set` takes `O(1)` normally and `O(N)` in the worst case. It first initialize the unordered set using the `nums` and for each value in the `nums`, it removes the longest consecutive sequence and update `max_len`. 

```C++
int longestConsecutive(vector<int>& nums) {
    int max_len = 0;
    unordered_set<int> dict(nums.begin(), nums.end());
    
    for(int num : nums) {
        if(dict.find(num) == dict.end())
            continue;
        
        dict.erase(num);
        int prev = num - 1, next = num + 1;
        while(dict.find(prev) != dict.end())
            dict.erase(prev--);
        while(dict.find(next) != dict.end())
            dict.erase(next++);
    
        max_len = max<int>(max_len, next - prev - 1);
    }
    
    return max_len;
}
```