# 1231. Divide Chocolate

## Description

You have one chocolate bar that consists of some chunks. Each chunk has its own sweetness given by the array `sweetness`.

You're going to share this chocolate with `K` friends, so you need to cut `K` times to get `K + 1` pieces, each of which is made up of a series of small pieces.

Being generous, you will eat the piece with the **minimum** total sweetness and give the other pieces to your friends.

Find the **maximum** total sweetness of the piece you can get by cutting the chocolate bar optimally.

**Example:**

```
Input: sweetness = [1,2,3,4,5,6,7,8,9], K = 5
Output: 6
Explanation: You can divide the chocolate to [1,2,3], [4,5], [6], [7], [8], [9]
```

## Classic Solution
Using binary search to find the optimal partition.

In each iteration of binary search, we test if the current partition can produce `K+1` chocolates with at least `mid` sweetness.

```C++
class Solution {
public:
    int maximizeSweetness(vector<int> &sweetness, int K) {
        int totalSweetness = 0;
        for(auto& sweet : sweetness)
            totalSweetness += sweet;
        
        int low = 0;
        int high = totalSweetness;
        while(low < high) {
            int mid = high - ((high - low) >> 1);

            if(canPartition(sweetness, K, mid))
                low = mid;
            else
                high = mid - 1;
        }
        return high;
    }
private:
    bool canPartition(vector<int> &sweetness, int K, int mid) {
        int currSweetness = 0;
        int count = 0;
        for(auto& sweet : sweetness) {
            currSweetness += sweet;
            if(currSweetness >= mid) {
                currSweetness = 0;
                count++;
            }
        }

        if(count > K)
            return true;
        else
            return false;
    }
};
```