# 875. Koko Eating Bananas

## Description
Koko loves to eat bananas. There are `n` piles of bananas, the `ith` pile has `piles[i]` bananas. The guards have gone and will come back in `h` hours.

Koko can decide her bananas-per-hour eating speed of `k`. Each hour, she chooses some pile of bananas and eats `k` bananas from that pile. If the pile has less than `k` bananas, she eats all of them instead and will not eat any more bananas during this hour.

Koko likes to eat slowly but still wants to finish eating all the bananas before the guards return.

Return the minimum integer `k` such that she can eat all the bananas within `h` hours.

**Example:**
```
Input: piles = [3,6,7,11], h = 8
Output: 4
```
## My Solution

A standard binary search solution.

However, here're some corner cases we need to be careful:

- The initial left pointer `low` should be set to `1` instead of `0`, otherwise it will cause divide by zero exception.

- When we use `ceil()` in `cmath` library, we need to make sure the input parameter is `float` or `double` type. (In this problem, we should use `double` for the sake of accuracy)

- Using `(pile + speed - 1) / speed` is a much efficient way of performing ceiling operation without using library function.


```C++
class Solution {
public:
    int minEatingSpeed(vector<int>& piles, int h) {
        long low = 1;
        long high = 1e13;
        
        while(low < high) {
            long mid = low + ((high - low) >> 1);
            if(canEat(piles, h, mid))
                high = mid;
            else
                low = mid + 1;
        }
        return static_cast<int>(high);
    }
private:
    bool canEat(vector<int>& piles, int h, long speed) {
        int count = 0;
        for(auto& pile : piles) {
            // count += ceil(static_cast<double>(pile) / speed);
            count += (pile + speed - 1) / speed;
        }
        if(count > h)
            return false;
        else
            return true;
    }
};
```