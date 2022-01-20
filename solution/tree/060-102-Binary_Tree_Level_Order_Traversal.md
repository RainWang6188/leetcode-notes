# 102. Binary Tree Level Order Traversal

## Description
Given the root of a binary tree, return the level order traversal of its nodes' values. (i.e., from left to right, level by level).

**Example:**
```
Input: root = [3,9,20,null,null,15,7]
Output: [[3],[9,20],[15,7]]
```
## My Solution
### 1. DFS
This question can be solved in a recursive way.

We use a variable `level` to record the level of current node, which helps to find where to put this value in the result vector.

Then, traverse the tree using DFS, appending the value to the corresponding vector according to the value of `level`.

```C++
vector<vector<int>> levelOrder(TreeNode* root) {
    int level = 0;
    vector<vector<int>> res;
    traverse(root, level, res);
    return res;
}

void traverse(TreeNode *root, int level, vector<vector<int>> &res) {
    if(!root)
        return;
    
    if(res.size() < level + 1)
        res.push_back({root->val});
    else
        res[level].push_back(root->val);
    
    traverse(root->left, level + 1, res);
    traverse(root->right, level + 1, res);
}
```
### 2. BFS
Here's an iterative version using a queue.

The basic idea is BFS. We search the entire level beforing continuing to the next level.

```C++
vector<vector<int>> levelOrder(TreeNode* root) {
    vector<vector<int>> res;
    queue<TreeNode*> q;
    
    if(!root)
        return res;
    
    q.push(root);
    while(!q.empty()) {
        vector<int> current_level;
        for(int i = 0, n = q.size(); i < n; i++) {
            TreeNode *node = q.front();
            q.pop();
            
            current_level.push_back(node->val);
            if(node->left)
                q.push(node->left);
            if(node->right)
                q.push(node->right);
        }
        res.push_back(current_level);
    }
    return res;
}
```