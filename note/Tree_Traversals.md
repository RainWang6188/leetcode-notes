# Tree Traversals
In this post, we will discuss 4 ways to traverse a tree.

## 1. Preorder Traversal
### Recursive Version
```C++
vector<int> preorderTraversal(TreeNode* root) {
    vector<int> res;
    preorder(root, res);
    return res;
}

void preorder(TreeNode* root, vector<int>& res) {
    if(!root)
        return;
    
    res.push_back(root->val);
    preorder(root->left, res);
    preorder(root->right, res);
}
```
### Iterative Version
```C++
vector<int> preorderTraversal(TreeNode* root) {
    vector<int> res;
    if(!root)
        return res;
    
    stack<TreeNode*> stk;
    stk.push(root);
    TreeNode *cur = root;
    while(!stk.empty()) {
        cur = stk.top();
        stk.pop();
        
        res.push_back(cur->val);
        
        if(cur->right)
            stk.push(cur->right);
        if(cur->left)
            stk.push(cur->left);
    }
    
    return res;
}
```
## 2. Inorder Traversal
### Recursive Version
```C++
vector<int> inorderTraversal(TreeNode* root) {
    vector<int> res;
    inorder(root, res);
    return res;
}

void inorder(TreeNode* root, vector<int>& res) {
    if(!root)
        return;
    
    inorder(root->left, res);
    res.push_back(root->val);
    inorder(root->right, res);
}
```
### Iterative Version
```C++
vector<int> inorderTraversal(TreeNode* root) {
    vector<int> res;
    if(!root)
        return res;
    
    stack<TreeNode*> stk;
    TreeNode *cur = root;
    while(cur || !stk.empty()) {
        while(cur) {
            stk.push(cur);
            cur = cur->left;
        }
        
        cur = stk.top();
        stk.pop();
        res.push_back(cur->val);
        cur = cur->right;
    }
    return res;
}
```
## 3. Postorder Traversal

### Recursive Version
```C++
vector<int> postorderTraversal(TreeNode* root) {
    vector<int> res;
    postorder(root, res);
    return res;
}

void postorder(TreeNode* root, vector<int>& res) {
    if(!root)
        return;
    
    postorder(root->left, res);
    postorder(root->right, res);
    res.push_back(root->val);
}
```
### Iterative Version
We have discussed iterative inorder and iterative preorder traversals. In this section, iterative postorder traversal is discussed, which is more complex than the other two traversals (due to its nature of **non-tail recursion**, there is an extra statement after the final recursive call to itself). 

Postorder traversal can easily be done using two stacks, though. The idea is to push reverse postorder traversal to a stack. Once we have the reversed postorder traversal in a stack, we can just pop all items one by one from the stack and print them; this order of printing will be in postorder because of the LIFO property of stacks. Now the question is, how to get reversed postorder elements in a stack – the second stack is used for this purpose. For example, in the following tree, we need to get 1, 3, 7, 6, 2, 5, 4 in a stack. If take a closer look at this sequence, we can observe that this sequence is very similar to the preorder traversal. **The only difference is that the right child is visited before left child, and therefore the sequence is “root right left” instead of “root left right”**. 

So, we can do something like iterative preorder traversal with the following differences: 

a) Instead of printing an item, we push it to a stack. 

b) We push the left subtree before the right subtree.
Following is the complete algorithm. 

After step 2, we get the reverse of a postorder traversal in the second stack. We use the first stack to get the correct order. 

```C++
vector<int> postorderTraversal(TreeNode* root) {
    vector<int> res;
    if(!root)
        return res;
    
    stack<TreeNode*> stk;
    stack<int> rev;
    
    stk.push(root);
    TreeNode *cur = root;
    
    while(!stk.empty()) {
        cur = stk.top();
        stk.pop();
        rev.push(cur->val);
        
        if(cur->left)
            stk.push(cur->left);
        if(cur->right)
            stk.push(cur->right);
    }
    
    while(!rev.empty()) {
        int value = rev.top();
        rev.pop();
        res.push_back(value);
    }
    return res;
}
```
## 4. Levelorder Traversal
### Recursive Version
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
### Iterative Version

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