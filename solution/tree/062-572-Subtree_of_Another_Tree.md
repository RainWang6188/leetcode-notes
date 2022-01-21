# 572. Subtree of Another Tree

## Description
Given the roots of two binary trees `root` and `subRoot`, return `true` if there is a subtree of `root` with the same structure and node values of `subRoot` and `false` otherwise.

A subtree of a binary tree tree is a tree that consists of a node in tree and all of this node's descendants. The tree tree could also be considered as a subtree of itself.

**Example:**
```
Input: root = [3,4,5,1,2], subRoot = [4,1,2]
Output: true
```

## My Solution
It's easier to solve this question in a recursive way.

As you can see, this question is almost the same as [Same Tree](https://leetcode.com/problems/same-tree/) problem, where we want to check if two trees are identical.

In this problem, if `subRoot` is a subtree of `root`, then `subRoot` must be identical to some subtrees in `root`. As a result, the code is easy to implement.

```C++
bool isSubtree(TreeNode* root, TreeNode* subRoot) {
    if(!root) return false;
    return isIdentical(root, subRoot) || isSubtree(root->left, subRoot) || isSubtree(root->right, subRoot);
}

bool isIdentical(TreeNode *root1, TreeNode *root2) {
    if(!root1 && !root2)
        return true;
    else if(root1 && root2)
        return root1->val == root2->val && isIdentical(root1->left, root2->left) && isIdentical(root1->right, root2->right);
    else
        return false;
}
```
