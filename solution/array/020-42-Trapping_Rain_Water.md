# 42. Trapping Rain Water

## Description
Given `n` non-negative integers representing an elevation map where the width of each bar is `1`, compute how much water it can trap after raining.
## My Solution
I used two pointers to traverse the whole array. However, my solution focus on only one block (1*1), and increment the total amount by 1 each time. I scan the block from left to right and dottom to up. To avoid unnecessary search, I start from the minimum height of the array. But it will still receive a [TLE](https://leetcode.com/submissions/detail/605404375/) error when encounter large testcases.

``` C++
int trap(vector<int>& height) {
    int min_height = *min_element(height.begin(), height.end());
    
    int total = 0;
    int current_height = min_height;
    
    while(1) {
        current_height++;
        bool no_block = true; 
        bool begin = false;
        int current_count = 0;
        int current_total = 0;
        
        for(int i = 0; i < height.size(); i++) {
            if(height[i] >= current_height) {
                no_block = false;
                if(!begin) {
                    begin = true;
                    current_count = 0;
                }
                else {
                    current_total += current_count;
                    current_count = 0;
                }
            }
            else {
                if(begin) 
                    current_count++;                        
            }
        }
        
        total += current_total;
        if(no_block)
            break;
    }
    
    return total;
}
```


## Classic Solution
This solution is much smarter: it scans one strip (width=1) each time. It maintain two pointers: `left` and `right`, and two values `left_max` and `right_max`, which stores the maximum height of left/right so far. When the left height is smaller than the right height (i.e. `height[left] < height[right]`), we don't have to consider `height[right]` when computing the amount of water than can be contained, and vice versa.

```C++
int trap(vector<int>& height) {
    int total = 0;
    int left = 0, right = height.size() - 1;
    int left_max = 0, right_max = 0;
    
    while(left < right) {
        if(height[left] <= height[right]) {
            if(height[left] > left_max)
                left_max = height[left];
            else
                total += left_max - height[left];
            left++;
        }
        else {
            if(height[right] > right_max)
                right_max = height[right];
            else
                total += right_max - height[right];
            right--;
        }
    }
    
    return total;
}
```