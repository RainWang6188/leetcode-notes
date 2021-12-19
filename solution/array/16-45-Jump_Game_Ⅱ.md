# 45. Jump Game Ⅱ
## Description
Given an array of non-negative integers `nums`, you are initially positioned at the first index of the array.

Each element in the array represents your maximum jump length at that position.

Your goal is to reach the last index in the minimum number of jumps.

You can assume that you can always reach the last index.
## My Solution
Traverse the array backwards, and at each index `i`, record the minimum number of jumps from `i` to `nums.size()-1`. Keep updating the `jump` vector, and `jump[0]` would be the result.

```C++
int jump(vector<int>& nums) {
    vector<int> jump(nums.size(), 0);
    
    for(int i = nums.size() - 1; i >= 0; i--) {
        int min_jump;
        if(i == nums.size() - 1)
            min_jump = 0;
        else if(i + nums[i] >= nums.size() - 1)
            min_jump = 1;
        else 
            min_jump = nums.size();
        
        for(int j = i + 1; j <= min<int>(nums.size() - 1, nums[i] + i); j++) {
            min_jump = min<int>(min_jump, jump[j] + 1);
        }
        
        jump[i] = min_jump;
    }
    
    return jump[0];
}
```

## Classic Solution

### 1. DP
This problem can be solved by dynamic programming. However, I decide to practice that sort of problems in future section, so I would leave it blank currently and try to solve it by DP afterwards.

The related solution can be viewed [here](https://leetcode.com/problems/jump-game-ii/discuss/1192401/Easy-Solutions-w-Explanation-or-Optimizations-from-Brute-Force-to-DP-to-Greedy-BFS).

### 2. Greedy BFS
We can iterate over all indices maintaining the furthest reachable position from current index - `maxReachable` and currently furthest reached position - `lastJumpedPos`. Everytime we will try to update `lastJumpedPos` to furthest possible reachable index - `maxReachable`.

Updating the `lastJumpedPos` separately from `maxReachable` allows us to maintain track of minimum jumps required. Each time `lastJumpedPos` is updated, jumps will also be updated and store the minimum jumps required to reach `lastJumpedPos` (On the contrary, updating jumps with `maxReachable` won't give the optimal (minimum possible) value of jumps required).

We will just return it as soon as `lastJumpedPos` reaches(or exceeds) last index.
```C++
int jump(vector<int>& nums) {
	int n = size(nums), i = 0, maxReachable = 0, lastJumpedPos = 0, jumps = 0;
	while(lastJumpedPos < n - 1) {  // loop till last jump hasn't taken us till the end
		maxReachable = max(maxReachable, i + nums[i]);  // furthest index reachable on the next level from current level
		if(i == lastJumpedPos) {			  // current level has been iterated & maxReachable position on next level has been finalised
			lastJumpedPos = maxReachable;     // so just move to that maxReachable position
			jumps++;                          // and increment the level
	// NOTE: jump^ only gets updated after we iterate all possible jumps from previous level
	//       This ensures jumps will only store minimum jump required to reach lastJumpedPos
		}            
		i++;
	}
	return jumps;
}
```
