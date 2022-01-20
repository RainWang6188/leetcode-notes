# 124. Binary Tree Maximum Path Sum
## Description
A **path** in a binary tree is a sequence of nodes where each pair of adjacent nodes in the sequence has an edge connecting them. A node can only appear in the sequence **at most once**. Note that the path does not need to pass through the root.

The **path sum** of a path is the sum of the node's values in the path.

Given the root of a binary tree, return the maximum **path sum** of any **non-empty** path.


**Example:**
```
Input: root = [1,2,3]
Output: 6
Explanation: The optimal path is 2 -> 1 -> 3 with a path sum of 2 + 1 + 3 = 6.
```
## Classic Solution
We can solve this question in a recursive way. Note that here `max_sum` is a member variable which can be accessed by any member function.

For any parent node, we can see that here are two paths
-  `left -> parent -> right`
        
    In this case, we can choose to add `left` (`right`) depending on whether it contributes a gain (i.e. `left->val  > 0`). 

- `left/right -> parent -> ...`

    In this case, only one of the left or right path can be choosed, so we will add the one with more gain.

```
        _ left
       |
parent +
       |_ right
```

```C++
int max_sum;

int maxPathSum(TreeNode* root) {
    max_sum = INT_MIN;
    get_max_gain(root);
    return max_sum;
}

int get_max_gain(TreeNode *root) {
    if(!root)
        return 0;
    
    int left_sum = max(get_max_gain(root->left), 0);
    int right_sum = max(get_max_gain(root->right), 0);
    max_sum = max(max_sum, left_sum + right_sum + root->val);   
    return root->val + max(left_sum, right_sum);
}
```