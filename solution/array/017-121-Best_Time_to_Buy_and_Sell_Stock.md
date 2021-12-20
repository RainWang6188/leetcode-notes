# 121. Best Time to Buy and Sell Stock
## Description
You are given an array prices where `prices[i]` is the price of a given stock on the $i^{th}$ day.

You want to maximize your profit by choosing a **single day** to buy one stock and choosing a **different day in the future** to sell that stock.

Return the maximum profit you can achieve from this transaction. If you cannot achieve any profit, return `0`.
## My Solution
### 1. DP
My original solution is using dynamic programming to solve this question, since the state transition function can be easily derived as:

`profit[i] = max(profit[i], profit[j] + (prices[j] - prices[i]))`, where `i < j < n`

However, it will receive a [TLE](https://leetcode.com/submissions/detail/604232975/) error in leetcode OJ.
```C++
int maxProfit(vector<int>& prices) {
    int n = prices.size();
    int max_profit = 0;
    vector<int> profit(n, 0);
    
    for(int i = n - 2; i >= 0; i--) {
        for(int j = i + 1; j < n; j++) {
            int diff = prices[j] - prices[i];
            profit[i] = max<int>(profit[i], profit[j] + diff);
        }
        max_profit = max<int>(max_profit, profit[i]);
    }
    
    return max_profit;
}
```

## One Pass DP
This problem can be solved in one pass. Since we are only asked to give the maximum profit without keeping the solution, so we can simply keep updating the maximum profit as well as the minimum prices so far. 

```C++
int maxProfit(vector<int>& prices) {
    if(prices.size() < 2)
        return 0;
    
    int max_profit = 0;
    int min_prices = prices[0];
    
    for(int i = 1; i < prices.size(); i++) {
        min_prices = min<int>(min_prices, prices[i]);
        max_profit = max<int>(max_profit, prices[i] - min_prices);
    }
    
    return max_profit;
}
```
This code can be optimized, we can update either `min_prices` or `max_profit` depending on `prices[i] > prices[i-1]` or not.

## Classic Solution - Kadane's Algorithm
If the interviewer twists the input, instead of giving the originial straight forward prices, he gives the **difference array of prices**, for example, for `{1, 7, 4, 11}`, if he gives `{0, 6, -3, 7}`, you might end up being confused.

Here, this problem can be viewed as a [Maximum Subarray Problem](https://en.wikipedia.org/wiki/Maximum_subarray_problem), because we need to calculate the largest subarray sum to get the most profit. Kadane's Algorithm is an $O(N)$-time solution which employs optimal substructure, so can be viewed as a trivial example of dynamic programming.

```c++
int maxProfit(vector<int>& prices) {
    if(prices.size() < 2)
        return 0;
    
    int max_profit = 0;
    int max_current_profit = 0;

    for(int i = 1; i < prices.size(); i++) {
        max_current_profit = max<int>(0, max_current_profit += prices[i] - prices[i-1]);
        max_profit = max<int>(max_profit, max_current_profit);
    }
    
    return max_profit;
}
```

