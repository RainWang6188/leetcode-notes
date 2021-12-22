# 11. Container with Most Water
## Description
You are given an integer array `height` of length `n`. There are `n` vertical lines drawn such that the two endpoints of the ith line are `(i, 0)` and `(i, height[i])`.

Find two lines that together with the x-axis form a container, such that the container contains the most water.

Return the maximum amount of water a container can store.

**Notice** that you may not slant the container.

**example**
```
Input: height = [1,8,6,2,5,4,8,3,7]
Output: 49
Explanation: The above vertical lines are represented by array [1,8,6,2,5,4,8,3,7]. In this case, the max area of water (blue section) the container can contain is 49.
```

## My Solution
Use two pointers to traverse the array, based on brute force. To cut down the redundant branches, it will check if the current left line is larger than the maximum left line so far when scanning to the right, if it does not, then it is unnecessary to go down along that branch, since it will never exceed the maximum amount of water (because of the smaller width).

Time Complexity: $O(N^2)$, Space Complexity: $O(1)$.

```c++
int maxArea(vector<int>& height) {
    int max_amount = 0;
    int max_height = 0;
    
    for(int i = 0; i < height.size() - 1; i++) {
        if(height[i] <= max_height)
            continue;
        else {
            max_height = height[i];
            for(int j = i + 1; j < height.size(); j++) {
                int current_amount = (j - i) * min<int>(height[i], height[j]);
                max_amount = max<int>(max_amount, current_amount);
            }
        }
    }
    
    return max_amount;
}
```

## Classic Solution
Almost same idea as me, but it sets the two pointers moving from the end of the array to the middle. However, moving which pointer after one iteration is tricky: we always move the index of shorter line, since it's more likely to form a larger container after the move.

```C++
int maxArea(vector<int>& height) {
    int i = 0, j = height.size() - 1;
    int wide_height = min<int>(height[i], height[j]);
    int max_amount = (j - i) * wide_height;
    
    while(i < j) {
        max_amount = max<int>(max_amount, (j - i) * min<int>(height[i], height[j]));
        
        if(height[i] < height[j])
            i++;
        else
            j--;
    }
    
    return max_amount;
}
```