# 1354. Construct Target Array With Multiple Sums

## Description

You are given an array `target` of `n` integers. From a starting array `arr` consisting of `n` 1's, you may perform the following procedure :

- let `x` be the sum of all elements currently in your array.
- choose index `i`, such that `0 <= i < n` and set the value of `arr` at index `i` to `x`.

You may repeat this procedure as many times as needed.

Return `true` if it is possible to construct the `target` array from `arr`, otherwise, return `false`.

**Example:**
```
Input: target = [9,3,5]
Output: true
Explanation: Start with arr = [1, 1, 1] 
[1, 1, 1], sum = 3 choose index 1
[1, 3, 1], sum = 5 choose index 2
[1, 3, 5], sum = 9 choose index 0
[9, 3, 5] Done
```

## Classic Solution

[Here] is a great explanation.

```C++
bool isPossible(vector<int>& A) {
    long total = 0;
    int n = A.size(), a;
    priority_queue<int> pq(A.begin(), A.end());
    for (int a : A)
        total += a;
    while (true) {
        a = pq.top(); pq.pop();
        total -= a;
        // left eles are all 1s || corner case {1, 10000}
        if (a == 1 || total == 1)
            return true;
        
        // total is too large to be added to a
        // || total == 0 corner case, only one element in array
        // || a is divisible by total -> can't add extra 1
        if (a < total || total == 0 || a % total == 0)
            return false;
        a %= total;
        total += a;
        pq.push(a);
    }
}
```