# 189. Rotate Array
## Description
Given an array, rotate the array to the right by `k` steps, where `k` is non-negative.

**Example 1:**
```
Input: nums = [1,2,3,4,5,6,7], k = 3
Output: [5,6,7,1,2,3,4]
Explanation:
rotate 1 steps to the right: [7,1,2,3,4,5,6]
rotate 2 steps to the right: [6,7,1,2,3,4,5]
rotate 3 steps to the right: [5,6,7,1,2,3,4]
```
**Example 2:**
```
Input: nums = [-1,-100,3,99], k = 2
Output: [3,99,-1,-100]
Explanation: 
rotate 1 steps to the right: [99,-1,-100,3]
rotate 2 steps to the right: [3,99,-1,-100]
```

## My Solution
### Solution 1
Simply make extra copy of the array and do the rotation.

Time Complexity: `O(N)`; Space Complexity: `O(N)`.
```C++
void rotate(vector<int>& nums, int k) {
    vector<int> temp = nums;
    
    for(int i = 0; i < nums.size(); i++){
        temp[(i+k) % nums.size()] = nums[i];
    }
    
    for(int i = 0; i < temp.size(); i++)
        nums[i] = temp[i];
}
```

### Solution 2
An in-plcae solution. We start from the `nums[0]` and trace the rotation. Note that if `nums.size()` $\equiv$ `k`, that trace would be a loop. So we need to jump outside and start another loop until we have rotate `nums.size()` times.

Time Complexity: `O(N)`; Space Complexity: `O(1)`.
```C++
void rotate(vector<int>& nums, int k) {
    if(nums.size() == 0)
        return;

    int count = 0;
    int start_index = 0;
    int current_index = 0;
    int target = nums[0];

    while(count < nums.size()){
        do {
        int next_index = (current_index + k) % nums.size();
        int source = nums[next_index];
        nums[next_index] = target;

        count++;
        target = source;
        current_index = next_index;
        } while(current_index != start_index);

        current_index = ++start_index;
        target = nums[current_index];
    }
}
```

## Classic Solution
`reverse()` operation acts like minus to index. For example, the $i^{th}$ element will become the $(n-i)^{th}$ element after reversal. (Suppose `n = nums.size()`)
```C++
void rotate(vector<int>& nums, int k) {
    k %=nums.size();
    reverse(nums.begin(), nums.end() - k);
    reverse(nums.end() - k, nums.end());
    reverse(nums.begin(), nums.end());
}
```
Let's examine the index step by step:
1) `reverse(nums.begin(), nums.end() - k)`

    Reverse the first `n-k` numbers.

    After reversal, for any $i \in [0, n - k)$, $i^{th}$ element becomes $(n-k-i)^{th}$ element. 

2) `reverse(nums.end()-k, nums.end())`

    Reverse the last $k$ elements.

    After reversal, for any $i \in [0, k)$, $(n-k+i)^{th}$ element becomes $(n-i)^{th}$ element.
3) `reverse(nums.begin(), nums.end())`

    Reverse all the elements.

    After reversal:
    - for any $i\in[0,n-k)$, $i^{th}$ element becomes $(i+k)^{th}$ element.
    - for any $i\in[n-k, n)$, $(n-k+i)^{th}$ element becomes $i^{th}$ element.

And it achieves "rotate the array by `k` steps".

There are many other solutions, which can be viewed [here](https://leetcode.com/problems/rotate-array/discuss/54277/Summary-of-C%2B%2B-solutions).