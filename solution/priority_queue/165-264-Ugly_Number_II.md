# 264. Ugly Number II

## Description

An ugly number is a positive integer whose prime factors are limited to `2`, `3`, and `5`.

Given an integer `n`, return the `nth` ugly number.

**Example:**
```
Input: n = 10
Output: 12
Explanation: [1, 2, 3, 4, 5, 6, 8, 9, 10, 12] is the sequence of the first 10 ugly numbers.
```

## My Solution
Traverse to find the `nth` ugly number. It is not efficient.
```C++
class Solution {
public:
    int nthUglyNumber(int n) {
        int curr = 1;
        while(1) {
            if(isUgly(curr)) {
                if(!--n)
                    return curr;
            }
            curr++;
        }
        return -1;
    }
private:
    bool isUgly(int n) {
        for(int i = 2; i < 6; i++)
            while(n % i == 0)
                n /= i;
        return n == 1;
    }
};
```

## Classic Solution

### 1. Dynamic Programming

Let's say we have the first 3 ugly numbers `1, 2, 3`. What would be the next number? 

Given the definition, the next number has to be the the **smallest** number among `2*(1,2,3)`, `3*(1,2,3)`, `5*(1,2,3)`. Obviously, the smallest number is `2 * 1 = 2`. But wait, `2` is already in there. 

So precisely speaking, the next number has to be the the smallest number among all the existing numbers multiplied by `2, 3, 5` that isn't in the list already. 

Now, we can traverse all numbers and maintain a hashset if we want, but it would become $O(N^2)$. Good news is that we can solve this in a DP-like approach. First, we assume there is only one number in the list, which is `1`. The next number is `Min(2 * 1, 3 * 1, 5 * 1)=2` and it is not in the list. 

Because we have already considered `2*1` (we move the pointer to its next position, which is `2`), now we only need to consider `2 * 2`, `3 * 1`, `5 * 1` in the next iteration. This way, we don't have to worry about finding a number that is already in the list.

```C++
int nthUglyNumber(int n) {
    if(n == 1)
        return 1;
    vector<int> dp(n, 0);
    dp[0] = 1;
    int pointer2 = 0;
    int pointer3 = 0;
    int pointer5 = 0;
    for(int i = 1; i < n; i++) {
        dp[i] = min({dp[pointer2] * 2, dp[pointer3] * 3, dp[pointer5] * 5});
        if(dp[i] == dp[pointer2] * 2)
            pointer2++;
        if(dp[i] == dp[pointer3] * 3)
            pointer3++;
        if(dp[i] == dp[pointer5] * 5)
            pointer5++;
    }
    return dp[n-1];
}
```

### 2. priority queue
skip the duplicates in each iteration.
```C++
int nthUglyNumber(int n) {
    priority_queue<long, vector<long>, greater<long>> pq;
    pq.push(1l);
    for(int i = 1; i < n; i++) {
        long curr = pq.top();
        while(!pq.empty() && pq.top() == curr)
            pq.pop();
        
        pq.push(2 * curr);
        pq.push(3 * curr);
        pq.push(5 * curr);
    }
    return pq.top();
}
```

### 3. std::set

Using set can avoid checking the duplicates, which is better than using priority queue.

```C++
int nthUglyNumber(int n) {
    set<long> uglies;
    uglies.insert(1);
    for(int i = 0; i < n - 1; i++) {
        long curr = *(uglies.begin());
        uglies.erase(curr);
        uglies.insert(curr * 2);
        uglies.insert(curr * 3);
        uglies.insert(curr * 5);
    }
    return *(uglies.begin());
}
```