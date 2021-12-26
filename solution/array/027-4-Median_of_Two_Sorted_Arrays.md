# 4. Median of Two Sorted Arrays

## Description
Given two sorted arrays `nums1` and `nums2` of size `m` and `n` respectively, return the median of the two sorted arrays.

The overall run time complexity should be `O(log (m+n))`.

**example**
```
Input: nums1 = [1,2], nums2 = [3,4]
Output: 2.50000
Explanation: merged array = [1,2,3,4] and median is (2 + 3) / 2 = 2.5.
```

## My Solution
I combined all the values in two vectors to the multiset `set1`. Since the elements in a set is sorted, so we can find the median of that easily.
```C++
double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
    multiset<int> set1(nums1.begin(), nums1.end());
    multiset<int> set2(nums2.begin(), nums2.end());
    
    set1.insert(set2.begin(), set2.end());
    
    multiset<int>::iterator it = set1.begin();
    double med = 0;
    int n = set1.size();
    if(n % 2) {
        for(int i = 0; i < (n - 1) / 2; i++)
            it++;
        med = static_cast<double>(*it);
    }
    else {
        for(int i = 0; i < n / 2 - 1; i++)
            it++;
        med = static_cast<double>(*(it++) + *it) / 2;
    }
    
    return med;
}
```

## Classic Solution
Here's an really clever solution using binary search. 

First let's cut A into two parts at a random position `i`:
```
      left_A             |        right_A
A[0], A[1], ..., A[i-1]  |  A[i], A[i+1], ..., A[m-1]
```
Since A has `m` elements, so there are `m+1` kinds of cutting( `i = 0 ~ m` ). And we know: `len(left_A) = i, len(right_A) = m - i`. Note: when `i = 0` , left_A is empty, and when `i = m` , right_A is empty.

With the same way, cut B into two parts at a random position j:
```
      left_B             |        right_B
B[0], B[1], ..., B[j-1]  |  B[j], B[j+1], ..., B[n-1]
```
Put left_A and left_B into one set, and put right_A and right_B into another set. Let's name them left_part and right_part :
```
      left_part          |        right_part
A[0], A[1], ..., A[i-1]  |  A[i], A[i+1], ..., A[m-1]
B[0], B[1], ..., B[j-1]  |  B[j], B[j+1], ..., B[n-1]
```
If we can ensure:
1) `len(left_part)` == `len(right_part)`
2) `max(left_part)` <= `min(right_part)`

then we divide all elements in {A, B} into two parts with equal length, and one part is always greater than the other. Then we can compute the median as follows:

$$
median=
\begin{cases}
(\max(left\_part ) + \min(right\_part))/2 & \text{if } (m+n) \text{ is even} \\
\max(left\_part) & \text{if } (m+n) \text{ is odd}
\end{cases}
$$

As a result, we can use binary search to find that partition.

```c++
double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
    //if nums1 size is greater then switch them so that nums1 is smaller than nums2.
    if(nums1.size() > nums2.size())
        return findMedianSortedArrays(nums2, nums1);
    int x = nums1.size();
    int y = nums2.size();
    
    int low = 0;
    int high = x;
    while(low <= high) {
        int partitionX = (low + high) / 2;
        int partitionY = (x + y + 1) / 2 - partitionX;
        
        //if partitionX is 0, it means nothing is there on the left side. Use -INF for maxLeftX
        //if partitionY is nums1.size(), it means there's nothing on the right side. Use +INF for minRightX
        int maxLeftX = (partitionX == 0) ? INT_MIN : nums1[partitionX - 1];
        int minRightX = (partitionX == x) ? INT_MAX : nums1[partitionX];

        int maxLeftY = (partitionY == 0) ? INT_MIN : nums2[partitionY - 1];
        int minRightY = (partitionY == y) ? INT_MAX : nums2[partitionY];

        if(maxLeftX <= minRightY && maxLeftY <= minRightX) {
            //We have partitioned array at correct place
            //Now get max of left elements and min of right elements to get the median in case of even length combined array size
            //or get max of left for odd length combined array size.
            if((x + y) % 2 == 0) {
                return static_cast<double>(max(maxLeftX, maxLeftY) + min(minRightX, minRightY)) / 2;
            } 
            else {
                return static_cast<double>(max(maxLeftX, maxLeftY));
            }
        }
        else if(maxLeftX > minRightY) { //We are too far on right side for partitionX, move left
            high = partitionX - 1;
        }
        else { //We are too far on left side, move right 
            low = partitionX + 1;
        }
    }
    return -1;
}
```
