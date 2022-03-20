# 337. House Robber III

## Description
The thief has found himself a new place for his thievery again. There is only one entrance to this area, called `root`.

Besides the `root`, each house has one and only one parent house. After a tour, the smart thief realized that all houses in this place form a binary tree. It will automatically contact the police if **two directly-linked houses were broken into on the same night**.

Given the root of the binary tree, return the maximum amount of money the thief can rob **without alerting the police**.

**Example:**
```
Input: root = [3,2,3,null,3,null,1]
Output: 7
Explanation: Maximum amount of money the thief can rob = 3 + 3 + 1 = 7.
```

## My Solution
My solution is naive, which take exponential time and will receive TLE in the leetcode OJ.

For each parent node `node`, we have two options: 
- Rob it
- Don't rob it

So for each case, we can recursively find the best choice:
- Rob it: then we can only rob the grandchildren of `node`, since they're not directly-linked.
    ```C++
    node->val + rob(node->left->left) + rob(node->left->right) + rob(node->right->left) + rob(node->right->right)
    ```

- Don't rob it: we can rob the children of `node`
    ```C++
    rob(node->left) + rob(node->right);
    ```

Here's the code:

```C++
int rob(TreeNode* root) {
    if(!root)
        return 0;
    
    int doRob = 0;
    if(root->left)
        doRob += rob(root->left->left) + rob(root->left->right);
    if(root->right)
        doRob += rob(root->right->left) + rob(root->right->right);
    
    return max(root->val + doRob, rob(root->left) + rob(root->right));
}
```

## Classic Solution

### 1. Optimization of My Solution
In the previous solution, we did a lot of redundant computation. 

For example, to obtain rob(root), we need rob(root.left), rob(root.right), rob(root.left.left), rob(root.left.right), rob(root.right.left), rob(root.right.right); but to get rob(root.left), we also need rob(root.left.left), rob(root.left.right), similarly for rob(root.right). 

So we can use a hash map to store the result to avoid recomputation.

```C++
class Solution {
public:
    int rob(TreeNode* root) {
        unordered_map<TreeNode*, int> dpDict;
        
        return tryRob(root, dpDict);
    }
private:
    int tryRob(TreeNode* root, unordered_map<TreeNode*, int>& dpDict) {
        if(!root)
            return 0;
        if(dpDict.find(root) != dpDict.end())
            return dpDict.find(root)->second;
        
        int doRob = root->val;
        if(root->left)
            doRob += tryRob(root->left->left, dpDict) + tryRob(root->left->right, dpDict);
        if(root->right)
            doRob += tryRob(root->right->left, dpDict) + tryRob(root->right->right, dpDict);
         
        int doNotRob = tryRob(root->left, dpDict) + tryRob(root->right, dpDict);
        int currMax = max(doRob, doNotRob);
        dpDict[root] = currMax;
        return currMax;
    }
};
```


### 2. Better Solution
In step I, we defined our problem as `rob(root)`, which will yield the maximum amount of money that can be robbed of the binary tree rooted at root. This leads to the DP problem summarized in step II.

Now let's take one step back and ask why we have overlapping subproblems. If you trace all the way back to the beginning, you'll find the answer lies in the way how we have defined `rob(root)`. As I mentioned, for each tree root, there are two scenarios: it is robbed or is not. `rob(root)` does not distinguish between these two cases, so "information is lost as the recursion goes deeper and deeper", which results in repeated subproblems.

If we were able to maintain the information about the two scenarios for each tree root, let's see how it plays out. Redefine `rob(root)` as a new function which will return an array of two elements, the first element of which denotes the maximum amount of money that can be robbed if root is not robbed, while the second element signifies the maximum amount of money robbed if it is robbed.

Let's relate `rob(root)` to `rob(root.left)` and `rob(root.right)`..., etc. For the 1st element of `rob(root)`, we only need to sum up the larger elements of `rob(root.left)` and `rob(root.right)`, respectively, since root is not robbed and we are free to rob its left and right subtrees. For the 2nd element of `rob(root)`, however, we only need to add up the 1st elements of `rob(root.left)` and `rob(root.right)`, respectively, plus the value robbed from `root` itself, since in this case it's guaranteed that we cannot rob the nodes of `root.left` and `root.right`.

As you can see, by keeping track of the information of both scenarios, we decoupled the subproblems and the solution essentially boiled down to a greedy one. Here is the program:
```C++
class Solution {
public:
    int rob(TreeNode* root) {
        auto [doRob, doNotRob] = tryRob(root);
        return max(doRob, doNotRob);
    }
    
private:
    pair<int, int> tryRob(TreeNode* root) {
        if(!root)
            return make_pair(0, 0);
        
        auto leftPair = tryRob(root->left);
        auto rightPair = tryRob(root->right);
        
        int doRob = root->val + leftPair.second + rightPair.second;
        int doNotRob = max(leftPair.first, leftPair.second) + max(rightPair.first, rightPair.second);
        
        return make_pair(doRob, doNotRob);
    }
};
```