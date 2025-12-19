## 1 Binary Tree Inorder Traversal

Given the root of a binary tree, return the inorder traversal of its nodes' values.

```cpp
class Solution {
public:
    void solve(TreeNode* root,vector<int>&res){
        if(!root)
        return ;
        solve(root->left,res);
        res.push_back(root->val);
        solve(root->right,res);
    }
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int>res;
        solve(root,res);
        return res;
    }
};
```

## 2 Binary Tree Preorder Traversal

Given the root of a binary tree, return the preorder traversal of its nodes' values.

```cpp
class Solution {
public:
    void solve(TreeNode* root,vector<int>&res){
        if(!root)
        return ;
        res.push_back(root->val);
        solve(root->left,res);
        solve(root->right,res);
    }
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int>res;
        solve(root,res);
        return res;
    }
};
```

## 3 Binary Tree Postorder Traversal

Given the root of a binary tree, return the postorder traversal of its nodes' values.

```cpp
class Solution {
public:
    void solve(TreeNode* root,vector<int>&res){
        if(!root)
        return ;
        solve(root->left,res);
        solve(root->right,res);
        res.push_back(root->val);
    }
    vector<int> postorderTraversal(TreeNode* root) {
        vector<int>res;
        solve(root,res);
        return res;
    }
};
```

## 4 Invert Binary Tree

Given the root of a binary tree, invert the tree, and return its root.

```cpp
class Solution {
public:
    TreeNode* solve(TreeNode*root){
        if(!root)
        return NULL;
        TreeNode*left=root->left;
        root->left=solve(root->right);
        root->right=solve(left);
        return root;
    }
    TreeNode* invertTree(TreeNode* root) {
        solve(root);
        return root;
    }
};
```
