# 42. Trapping Rain Water

## Description

Given `n` non-negative integers representing an elevation map where the width of each bar is `1`, compute how much water it can trap after raining.

**Example:**
![exp](https://assets.leetcode.com/uploads/2018/10/22/rainwatertrap.png)
```
Input: height = [0,1,0,2,1,0,1,3,2,1,2,1]
Output: 6
Explanation: The above elevation map (black section) is represented by array [0,1,0,2,1,0,1,3,2,1,2,1]. In this case, 6 units of rain water (blue section) are being trapped.
```

## Classic Solution

An solution using decreasing monotonic stack.

```C++
int trap(vector<int>& height) {
    int sum = 0;
    stack<int> indices;
    
    for(int i = 0; i < height.size(); i++) {
        while(!indices.empty() && height[i] > height[indices.top()]) {
            int curr = indices.top();
            indices.pop();
            if(indices.empty())
                break;
            int left = indices.top();
            int right = i;
            int currHeight = min(height[indices.top()], height[i]);
            sum += (right - left - 1) * (currHeight - height[curr]);
        }
        indices.push(i);
    }
    
    return sum;
}
```