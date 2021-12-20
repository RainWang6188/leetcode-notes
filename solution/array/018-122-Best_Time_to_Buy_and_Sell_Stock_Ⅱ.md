# 122. Best Time to Buy and Sell Stock Ⅱ

## Description
You are given an integer array prices where `prices[i]` is the price of a given stock on the $i^{th}$ day.

On each day, you may decide to buy and/or sell the stock. You can only hold **at most one** share of the stock at any time. However, you can buy it then immediately sell it on the **same day**.

Find and return the **maximum** profit you can achieve.
## My Solution - Greedy
My solution uses the greedy algorithm, which buys/sells the stock once we have chance to profit. 

If we find that the current stock price is higher than previous day, we would buy it and update the `current_profit`, otherwise we would sell it , updating the `max_profit`, and immediately buy it, setting `current_profit = 0`. 

At last, we need to add `current_profit` to `max_profit`, since the stock value may keep rising in the end so that we won't have chance to update the `max_profit` in the process.

```C++
int maxProfit(vector<int>& prices) {
    int max_profit = 0;
    int current_profit = 0;
    
    for(int i = 1; i < prices.size(); i++) {
        if(prices[i] > prices[i-1]) {
            current_profit += prices[i] - prices[i-1];
        }
        else {
            max_profit += current_profit;
            current_profit = 0;
        }
    }
    max_profit += current_profit;
    
    return max_profit;
}
```

## Classic Solution
The idea is the same as mine, but it only focus on increasing monotonous sequences.

```C++
int maxProfit(vector<int> &prices) {
    int ret = 0;
    for (size_t p = 1; p < prices.size(); ++p) 
      ret += max(prices[p] - prices[p - 1], 0);    
    return ret;
}
```
