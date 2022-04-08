# 313. Super Ugly Number

## Description

A super ugly number is a positive integer whose prime factors are in the array `primes`.

Given an integer `n` and an array of integers `primes`, return the `nth` super ugly number.

The `nth` super ugly number is guaranteed to fit in a 32-bit signed integer.

**Example 1:**
```
Input: n = 12, primes = [2,7,13,19]
Output: 32
Explanation: [1,2,4,7,8,13,14,16,19,26,28,32] is the sequence of the first 12 super ugly numbers given primes = [2,7,13,19].
```

## My Solution
The same algorithm as the previous question.

### 1. Dynamic Programming
```C++
int nthSuperUglyNumber(int n, vector<int>& primes) {
    vector<int> pointers(primes.size(), 0);
    vector<int> dp(n, INT_MAX);
    dp[0] = 1;
    for(int i = 1; i < n; i++) {
        for(int j = 0; j < pointers.size(); j++) {
            dp[i] = min(dp[i], dp[pointers[j]] * primes[j]);
        }

        for(int j = 0; j < pointers.size(); j++) {
            if(dp[i] == dp[pointers[j]] * primes[j])
                pointers[j]++;
        }
    }
    return dp.back();
}
```

### 2. priority_queue

```C++
int nthSuperUglyNumber(int n, vector<int>& primes) {
    priority_queue<long, vector<long>, greater<long>> pq;
    pq.push(1l);
    for(int i = 1; i < n; i++) {
        long curr = pq.top();
        while(!pq.empty() && pq.top() == curr)
            pq.pop();
        
        for(auto& prime : primes)
            pq.push(prime * curr);
    }
    return pq.top();
}
```
