# 33. Container with Most Water

## Description
You are given an integer array `height` of length `n`. There are `n` vertical lines drawn such that the two endpoints of the ith line are `(i, 0)` and `(i, height[i])`.

Find two lines that together with the x-axis form a container, such that the container contains the most water.

Return the maximum amount of water a container can store.

Notice that you may not slant the container.

**example**
```
Input: height = [1,8,6,2,5,4,8,3,7]
Output: 49
Explanation: The above vertical lines are represented by array [1,8,6,2,5,4,8,3,7]. In this case, the max area of water (blue section) the container can contain is 49.
```
## My Solution
My solution uses two pointers, search from both ends to the middle. Since the width is getting smaller, only when the height of the container gets larger will it be possible to exceeds the maximum water so far.

Owing to the fact that the water can be trapped is determined by the `min(height[i], height[j])`, we always move the index with smaller height to the middle. Again, we skip those whose height is smaller than its original height to avoid unnecessary check. 

Here's the code.

```C++
int maxArea(vector<int>& height) {
    int max_water = 0;
    int low = 0;
    int high = height.size() - 1;
    
    while(low < high) {
        int width = high - low;
        int current_height = min(height[low], height[high]);
        max_water = max(max_water, width * current_height);
        
        if(height[low] < height[high]) {
            while(low < high && height[low] <= current_height)
                low++;
        }
        else {
            while(low < high && height[high] <= current_height)
                high--;
        }
    }
    
    return max_water;
}
```