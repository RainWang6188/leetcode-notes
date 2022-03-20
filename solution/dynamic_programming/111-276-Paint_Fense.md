# 276. Paint Fense

## Description
There is a fence with `n` posts, each post can be painted with one of the `k` colors.

You have to paint all the posts such that **no more than two adjacent fence posts have the same color**.

Return the total number of ways you can paint the fence.

Note: `n` and `k` are non-negative integers.

**Example:**
```
Input: n = 3, k = 2
Output: 6
Explanation: Take c1 as color 1, c2 as color 2. All possible ways are:

  way       post1  post2  post3      
 -----      -----  -----  -----       
   1         c1     c1     c2
   2         c1     c2     c1
   3         c1     c2     c2
   4         c2     c1     c1  
   5         c2     c1     c2
   6         c2     c2     c1
```

## My Solution
For the base case:
- if `n == 0`, then the result is `0`.
- if `n == 1`, then the result is `k`.

For general cases, we use `same` and `diff` to denote the number of solutions until now if we paint the current fense in the same or different color with the previous one respectively.

Then, we can divide the problem into two cases:
- paint same: the number of solution is the same with previous `diff`.

- paint different: since the previous fense can be painted with same or different, the number of solution is `(same + diff) * (k - 1)`.


```C++
int numWays(int n, int k) {
    if(!n)
        return 0;

    int same = 0;
    int diff = k;
    for(int i = 1; i < n; i++) {
        int temp = same;
        same = diff;
        diff = (temp + diff) * (k - 1);
    }
    return same + diff;
}
```