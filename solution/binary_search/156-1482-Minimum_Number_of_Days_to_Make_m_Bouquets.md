# 1482. Minimum Number of Days to Make m Bouquets

## Description
You are given an integer array `bloomDay`, an integer `m` and an integer `k`.

You want to make `m` bouquets. To make a bouquet, you need to use `k` adjacent flowers from the garden.

The garden consists of `n` flowers, the `ith` flower will bloom in the `bloomDay[i]` and then can be used in exactly one bouquet.

Return the minimum number of days you need to wait to be able to make `m` bouquets from the garden. If it is impossible to make `m` bouquets return `-1`.


**Example:**
```
Input: bloomDay = [1,10,3,10,2], m = 3, k = 1
Output: 3
Explanation: Let us see what happened in the first three days. x means flower bloomed and _ means flower did not bloom in the garden.
We need 3 bouquets each should contain 1 flower.
After day 1: [x, _, _, _, _]   // we can only make one bouquet.
After day 2: [x, _, _, _, x]   // we can only make two bouquets.
After day 3: [x, _, x, _, x]   // we can make 3 bouquets. The answer is 3.
```

## My Solution

Using binary search to solve this problem.

There are only two conditions where it is impossible to make `m` bouquets:
- the `bloomDay` array is empty
- the total number of flowers is less than `m * k`

After excluding the above two conditions, we can use binary search to check if a specific day is valid to make `m` bouquets, and find the minimum day.

```C++
int minDays(vector<int>& bloomDay, int m, int k) {
    if(bloomDay.empty() || bloomDay.size() < m * k)
        return -1;
    
    long low = 0;
    long high = 1e9;
    while(low < high) {
        long mid = low + ((high - low) >> 1);
        int count = 0;
        int curr = 0;
        for(auto& day : bloomDay) {
            if(mid >= day)
                curr++;
            else {
                count += curr / k;
                curr = 0;
            }
        }
        count += curr / k;
        
        if(count < m)
            low = mid + 1;
        else
            high = mid;
    }
    return high;
}
```