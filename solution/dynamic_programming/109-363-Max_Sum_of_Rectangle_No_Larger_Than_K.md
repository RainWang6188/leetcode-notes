# 363. Max Sum of Rectangle No Larger Than K

## Description
Given an `m x n` matrix `matrix` and an integer `k`, return the max sum of a rectangle in the `matrix` such that its sum is no larger than `k`.

It is guaranteed that there will be a rectangle with a sum no larger than `k`.

**Example:**
```
Input: matrix = [[1,0,1],[0,-2,3]], k = 2
Output: 2
Explanation: Because the sum of the blue rectangle [[0, 1], [-2, 3]] is 2, and 2 is the max number no larger than k (k = 2).
```


## Classic Solution
This problem can be broken down into the following subproblems.

### 1. Max Sum of Rectangle in a 2D Matrix
This problem can be solved using [Kadane's algorithm](https://www.geeksforgeeks.org/largest-sum-contiguous-subarray/), which is simple and comprehensible.

When the problem transforms into a 2D version, what we need to do is fixing the left and right columns one by one and finding the maximum sum contiguous rows for every left and right column pair.

[Here](https://www.geeksforgeeks.org/maximum-sum-rectangle-in-a-2d-matrix-dp-27/)'s an overview of the algorithm.

### 2. Max Sum of Subarray No Largar than K
You can do this in  𝑂(𝑛log(𝑛)) 

First thing to note is that sum of subarray  (𝑖,𝑗]  is just the sum of the first  𝑗  elements less the sum of the first  𝑖  elements. Store these cumulative sums in the array cum. Then the problem reduces to finding  𝑖,𝑗  such that  𝑖<𝑗  and  𝑐𝑢𝑚[𝑗]−𝑐𝑢𝑚[𝑖]  is as close to  𝑘  but lower than it.

To solve this, scan from left to right. Put the  𝑐𝑢𝑚[𝑖]  values that you have encountered till now into a set. When you are processing  𝑐𝑢𝑚[𝑗]  what you need to retrieve from the set is the smallest number in the set such which is bigger than  𝑐𝑢𝑚[𝑗]−𝑘 . This lookup can be done in  𝑂(log𝑛)  using [lower_bound](http://www.cplusplus.com/reference/set/set/lower_bound/). Hence the overall complexity is  𝑂(𝑛log(𝑛)) .

> `set::lower_bound(const value_type& val)` returns the first iterator whose g**reater than or equal to** `val`.
> `set::upper_bound(const value_type& val)` returns the first iterator whose **greater** than `val`.

Here is a C++ function that does the job, assuming that K>0 and that the empty interval with sum zero is a valid answer. The code can be tweaked easily to take care of more general cases and to return the interval itself.
```C++
int best_cumulative_sum(int ar[],int N,int K) 
{ 
    set<int> cumset; 
    cumset.insert(0); 
 
    int best=0,cum=0; 
 
    for(int i=0;i<N;i++) 
    { 
        cum+=ar[i]; 
        set<int>::iterator sit=cumset.lower_bound(cum-K); 
        if(sit!=cumset.end())best=max(best,cum-*sit); 
        cumset.insert(cum); 
    } 
 
    return best; 
} 
```
**Note:** don't forget to insert a `0` into the set before the loop.


### 3. Solving this Question
As you can see in the finding the Max Sum of Rectangle in a 2D Matrix, we used Kadane's algorithm to find the maximum sum.

In this question we are asked to find the maximum sum no larger than `k`, where we should use the solution in part 2 instead of Kadane's algorithm.

Here's the code:

```C++
int maxSumSubmatrix(vector<vector<int>>& matrix, int k) {
    if(matrix.empty())
        return 0;
    
    int m = matrix.size();
    int n = matrix.front().size();
    int res = INT_MIN;
    
    for(int left = 0; left < n; left++) {
        vector<int> sum(m, 0);
        for(int right = left; right < n; right++) {
            for(int i = 0; i < m; i++) {
                sum[i] += matrix[i][right];
            }
            
            // find the max subarray no larger than K
            set<int> cum;
            cum.insert(0);
            int currSum = 0;
            int maxSum = INT_MIN;
            for(int i = 0; i < m; i++) {
                currSum += sum[i];
                
                auto it = cum.lower_bound(currSum - k);
                if(it != cum.end()) 
                    maxSum = max(maxSum, currSum - *it);
                cum.insert(currSum);
            }
            res = max(res, maxSum);
        }
    }
    return res;
}
```