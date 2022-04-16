# 538. Convert BST to Greater Tree

## Description

Given the `root` of a Binary Search Tree (BST), convert it to a Greater Tree such that every key of the original BST is changed to the original key plus the sum of all keys greater than the original key in BST.

As a reminder, a binary search tree is a tree that satisfies these constraints:

- The left subtree of a node contains only nodes with keys **less** than the node's key.
- The right subtree of a node contains only nodes with keys **greater** than the node's key.
- Both the left and right subtrees must also be binary search trees.

**Example 1:**

![exp](https://assets.leetcode.com/uploads/2019/05/02/tree.png)

```
Input: root = [4,1,6,0,2,5,7,null,null,null,3,null,null,null,8]
Output: [30,36,21,36,35,26,15,null,null,null,33,null,null,null,8]
```

## My Solution

For the recursive approach, we maintain some minor "global" state so each recursive call can access and modify the current total sum. Essentially, we ensure that the current node exists, recurse on the right subtree, visit the current node by updating its value and the total sum, and finally recurse on the left subtree. 

If we know that recursing on root.right properly updates the right subtree and that recursing on root.left properly updates the left subtree, then we are guaranteed to update all nodes with larger values before the current node and all nodes with smaller values after.


```C++
class Solution {
public:
    TreeNode* convertBST(TreeNode* root) {
        if(!root)
            return nullptr;
        
        root->right = convertBST(root->right);
        sum += root->val;
        root->val = sum;
        root->left = convertBST(root->left);
        
        return root;
    }
private:
    int sum;
};
```

## Classic Solution

This problem actually require us to perform a reverse in-order traversal on a BST.

The above solution perform the traversal in a reverse way. Here we'll solve it iteratively using a stack.

The only difference between a reverse in-order traversal and an ordinary in-order traversal is that we try to go **right** as far as possible during the pushing stack process in the reverse in-order traversal while we go **left** as far as possible in an ordinary in-order traversal.

Here's the implementation.

```C++
TreeNode* convertBST(TreeNode* root) {
    int sum = 0;
    stack<TreeNode*> stk;
    TreeNode *curr = root;
    while(curr || !stk.empty()) {
        while(curr) {
            stk.push(curr);
            curr = curr->right;
        }
        
        curr = stk.top();
        stk.pop();
        sum += curr->val;
        curr->val = sum;
        curr = curr->left;
    }
    return root;
}
```