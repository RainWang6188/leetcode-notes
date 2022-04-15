# 84. Largest Rectangle in Histogram

## Description

Given an array of integers `heights` representing the histogram's bar height where the width of each bar is `1`, return the area of the largest rectangle in the histogram.

**Example 1:**
```
Input: heights = [2,1,5,6,2,3]
Output: 10
Explanation: The above is a histogram where width of each bar is 1.
The largest rectangle is shown in the red area, which has an area = 10 units.
```


## My Solution

I checked each possible height, and update the maximum area for that height.

But the time complexity of this solution is $O(N^2)$, pretty slow.

```C++
int largestRectangleArea(vector<int>& heights) {
    int maxArea = 0;
    
    for(auto& h : heights) {
        bool start = false;
        int startIndex = -1;
        for(int i = 0; i < heights.size(); i++) {
            if(heights[i] >= h && !start) {
                start = true;
                startIndex = i;
            }
            if(start && heights[i] >= h) {
                maxArea = max(maxArea, h * (i - startIndex + 1));
            }
            else if(heights[i] < h)
                start = false;
        }
    }
    
    return maxArea;
}
```

## Classic Solution
> This solution excerpted from [here](https://leetcode-cn.com/problems/largest-rectangle-in-histogram/solution/84-by-ikaruga/)

This algorithm uses monotonic stack.

单调栈

单调栈分为单调递增栈和单调递减栈

1. 单调递增栈即栈内元素保持单调递增的栈
2. 同理单调递减栈即栈内元素保持单调递减的栈

操作规则（下面都以单调递增栈为例）
1. 如果新的元素比栈顶元素大，就入栈
2. 如果新的元素较小，那就一直把栈内元素弹出来，直到栈顶比新元素小

加入这样一个规则之后，会有什么效果
1. 栈内的元素是递增的
2. 当元素出栈时，说明这个新元素是出栈元素向后找第一个比其小的元素
3. 当元素出栈后，说明新栈顶元素是出栈元素向前找第一个比其小的元素

```C++
int largestRectangleArea(vector<int>& heights) {
    int maxArea = 0;
    heights.push_back(0);
    stack<int> indices;
    
    for(int i = 0; i < heights.size(); i++) {
        while(!indices.empty() && heights[i] < heights[indices.top()]) {
            int curr = indices.top();
            indices.pop();
            int left = indices.empty() ? -1 : indices.top();
            int right = i;
            maxArea = max(maxArea, (right - left - 1) * heights[curr]);
        }
        indices.push(i);
    }
    
    return maxArea;
}
```