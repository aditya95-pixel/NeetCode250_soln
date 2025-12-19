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

## 5 Maximum Depth of Binary Tree

Given the root of a binary tree, return its maximum depth.

A binary tree's maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.

```cpp
class Solution {
    int maxdep=0;
public:
    void solve(TreeNode*root,int dep){
        if(!root)
        return ;
        maxdep=max(maxdep,dep);
        solve(root->left,dep+1);
        solve(root->right,dep+1);
    }
    int maxDepth(TreeNode* root) {
        if(!root)
        return 0;
        solve(root,1);
        return maxdep;
    }
};
```

## 6 Diameter of Binary Tree

Given the root of a binary tree, return the length of the diameter of the tree.

The diameter of a binary tree is the length of the longest path between any two nodes in a tree. This path may or may not pass through the root.

The length of a path between two nodes is represented by the number of edges between them.

```cpp
class Solution {
    int diam=0;
public:
    int solve(TreeNode*root){
        if(!root)
        return 0;
        int l=solve(root->left),r=solve(root->right);
        diam=max(diam,l+r);
        return max(l,r)+1;
    }
    int diameterOfBinaryTree(TreeNode* root) {
        solve(root);
        return diam;
    }
};
```

## 7 Balanced Binary Tree

Given a binary tree, determine if it is height-balanced.

```cpp
class Solution {
    bool res=1;
public:
    int solve(TreeNode* root){
        if(!root)
        return 0;
        int l=solve(root->left),r=solve(root->right);
        if(l+1<r || r+1<l)
        res=0;
        return 1+max(l,r);
    }
    bool isBalanced(TreeNode* root) {
        solve(root);
        return res;
    }
};
```

## 8 Same Tree

Given the roots of two binary trees p and q, write a function to check if they are the same or not.

Two binary trees are considered the same if they are structurally identical, and the nodes have the same value.

```cpp
class Solution {
public:
    bool isSameTree(TreeNode* p, TreeNode* q) {
        if(!p && !q)
        return 1;
        else if(!p)
        return 0;
        else if(!q)
        return 0;
        else if(p->val!=q->val)
        return 0;
        else
            return isSameTree(p->left,q->left)&&isSameTree(p->right,q->right);
    }
};
```

## 9 Subtree of Another Tree

Given the roots of two binary trees root and subRoot, return true if there is a subtree of root with the same structure and node values of subRoot and false otherwise.

A subtree of a binary tree tree is a tree that consists of a node in tree and all of this node's descendants. The tree tree could also be considered as a subtree of itself.

```cpp
class Solution {
public:
    bool solve(TreeNode*root,TreeNode*subRoot){
        if(!root && !subRoot)
        return 1;
        else if(!root)
        return 0;
        else if(!subRoot)
        return 0;
        else if(root->val!=subRoot->val)
        return 0;
        else
        return solve(root->left,subRoot->left)&&solve(root->right,subRoot->right);
    }
    bool isSubtree(TreeNode* root, TreeNode* subRoot) {
        if(!root)
        return 0;
        if(root->val!=subRoot->val)
            return isSubtree(root->left,subRoot)||isSubtree(root->right,subRoot);
        else
            if(solve(root,subRoot))
            return 1;
            else
            return isSubtree(root->left,subRoot)||isSubtree(root->right,subRoot);
    }
};
```

## 10 Lowest Common Ancestor of a Binary Search Tree

Given a binary search tree (BST), find the lowest common ancestor (LCA) node of two given nodes in the BST.

According to the definition of LCA on Wikipedia: “The lowest common ancestor is defined between two nodes p and q as the lowest node in T that has both p and q as descendants (where we allow a node to be a descendant of itself).”

```cpp
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if(root->val<p->val && root->val<q->val)
        return lowestCommonAncestor(root->right,p,q);
        else if(root->val>p->val && root->val>q->val)
        return lowestCommonAncestor(root->left,p,q);
        else
        return root;
    }
};
```

## 11 Insert into a Binary Search Tree

You are given the root node of a binary search tree (BST) and a value to insert into the tree. Return the root node of the BST after the insertion. It is guaranteed that the new value does not exist in the original BST.

Notice that there may exist multiple valid ways for the insertion, as long as the tree remains a BST after insertion. You can return any of them.

```cpp
class Solution {
public:
    TreeNode* insertIntoBST(TreeNode* root, int val) {
        TreeNode*ptr=root,*preptr=NULL;
        while(1){
            if(ptr==NULL && preptr==NULL)
            return new TreeNode(val);
            else if(ptr==NULL && preptr->val>val)
            {preptr->left=new TreeNode(val);return root;}
            else if(ptr==NULL && preptr->val<val)
            {preptr->right=new TreeNode(val);return root;}
            else if(ptr->val>val)
            {
                preptr=ptr;
                ptr=ptr->left;
            }
            else if(ptr->val<val)
            {
                preptr=ptr;
                ptr=ptr->right;
            }
        }
        return root;
    }
};
```
