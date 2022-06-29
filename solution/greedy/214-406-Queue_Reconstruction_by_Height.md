# 406. Queue Reconstruction by Height

## Description

You are given an array of people, `people`, which are the attributes of some people in a queue (not necessarily in order). Each `people[i] = [hi, ki]` represents the `ith` person of height `hi` with exactly `ki` other people in front who have a height greater than or equal to `hi`.

Reconstruct and return the queue that is represented by the input array people. The returned queue should be formatted as an array queue, where `queue[j] = [hj, kj]` is the attributes of the `jth` person in the queue (`queue[0]` is the person at the front of the queue).


## Classic Solution

We first sort the `people` by non-ascending heights(first parameter) and inscreasing keys (second parameter).

The reason is as follows:

- non-ascending height: the shorter ones will not affect how those taller ones' position. So we place people from tallest to shortest, inserting each person one by one.

- increasing key: Since `[7, 0]` will be put in the front of `[7, 1]`, so we should place `[7, 0]` before `[7, 1]` to guarantee the correctness.

![demo](https://pic.leetcode-cn.com/1654443948-FwKdxL-9CCA8134-EE8E-4E73-8771-FBE8F47D4C7C_1_201_a.jpeg)

```C++
vector<vector<int>> reconstructQueue(vector<vector<int>>& people) {
    auto cmp = [](vector<int>& p1, vector<int>& p2) {
        if(p1[0] == p2[0])
            return p1[1] < p2[1];
        else
            return p1[0] > p2[0];
    };

    sort(people.begin(), people.end(), cmp);
    vector<vector<int>> q;
    for(const auto& p : people) {
        q.insert(next(q.begin(), p[1]), p);
    }
    return q;
}
```