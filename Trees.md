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

## 12 Delete Node in a BST

Given a root node reference of a BST and a key, delete the node with the given key in the BST. Return the root node reference (possibly updated) of the BST.

Basically, the deletion can be divided into two stages:

Search for a node to remove.
If the node is found, delete the node.

```cpp
class Solution {
public:
    int fetch(TreeNode* root,TreeNode*preptr){
        if(!root->left && preptr->val>root->val){
            int val=root->val;
            preptr->left=root->right;
            delete root;
            return val;
        }
        else if(!root->left && preptr->val<root->val){
            int val=root->val;
            preptr->right=root->right;
            delete root;
            return val;
        }
        return fetch(root->left,root);
    }
    TreeNode* deleteNode(TreeNode* root, int key) {
        if(!root)
        return NULL;
        if(root->val==key && !root->left && !root->right)
        return NULL;
        TreeNode*ptr=root,*preptr=NULL;
        while(ptr){
            if(ptr->val==key)
            break;
            else if(ptr->val>key)
            {
                preptr=ptr;
                ptr=ptr->left;
            }
            else{
                preptr=ptr;
                ptr=ptr->right;
            }
        }
        if(ptr==NULL)
        return root;
        else if(ptr->right==NULL && ptr->left==NULL && preptr->val>ptr->val){
            preptr->left=NULL;
        }
        else if(ptr->right==NULL && ptr->left==NULL && preptr->val<ptr->val){
            preptr->right=NULL;
        }
        else if(ptr->right)
            ptr->val=fetch(ptr->right,ptr);
        else if(ptr->left && preptr && ptr->val<preptr->val)
        {
            preptr->left=ptr->left;
            delete ptr;
        }
        else if(ptr->left && preptr && ptr->val>preptr->val)
        {
            preptr->right=ptr->left;
            delete ptr;
        }
        else if(ptr->left && !preptr){
            return ptr->left;
        }
        return root;
    }
};
```

## 13 Binary Tree Level Order Traversal

Given the root of a binary tree, return the level order traversal of its nodes' values. (i.e., from left to right, level by level).

```cpp
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>>res;
        if(!root)
        return res;
        queue<TreeNode*>q;
        q.push(root);
        while(!q.empty()){
            int sz=q.size();
            vector<int>temp;
            while(sz--){
                TreeNode* node=q.front();
                q.pop();
                temp.push_back(node->val);
                if(node->left)
                q.push(node->left);
                if(node->right)
                q.push(node->right);
            }
            res.push_back(temp);
        }
        return res;
    }
};
```

## 14 Binary Tree Right Side View

Given the root of a binary tree, imagine yourself standing on the right side of it, return the values of the nodes you can see ordered from top to bottom.

```cpp
class Solution {
public:
    vector<int> rightSideView(TreeNode* root) {
        vector<int>res;
        if(!root)
        return res;
        queue<TreeNode*>q;
        q.push(root);
        while(!q.empty()){
            int sz=q.size();
            int val;
            while(sz--){
                TreeNode* node=q.front();
                q.pop();
                val=node->val;
                if(node->left)
                q.push(node->left);
                if(node->right)
                q.push(node->right);
            }
            res.push_back(val);
        }
        return res;
    }
};
```

## 15 Construct Quad Tree

Given a n * n matrix grid of 0's and 1's only. We want to represent grid with a Quad-Tree.

Return the root of the Quad-Tree representing grid.

A Quad-Tree is a tree data structure in which each internal node has exactly four children. Besides, each node has two attributes:

val: True if the node represents a grid of 1's or False if the node represents a grid of 0's. Notice that you can assign the val to True or False when isLeaf is False, and both are accepted in the answer.
isLeaf: True if the node is a leaf node on the tree or False if the node has four children.

```txt
class Node {
    public boolean val;
    public boolean isLeaf;
    public Node topLeft;
    public Node topRight;
    public Node bottomLeft;
    public Node bottomRight;
}
```

We can construct a Quad-Tree from a two-dimensional area using the following steps:

If the current grid has the same value (i.e all 1's or all 0's) set isLeaf True and set val to the value of the grid and set the four children to Null and stop.
If the current grid has different values, set isLeaf to False and set val to any value and divide the current grid into four sub-grids as shown in the photo.
Recurse for each of the children with the proper sub-grid.

If you want to know more about the Quad-Tree, you can refer to the wiki.

Quad-Tree format:

You don't need to read this section for solving the problem. This is only if you want to understand the output format here. The output represents the serialized format of a Quad-Tree using level order traversal, where null signifies a path terminator where no node exists below.

It is very similar to the serialization of the binary tree. The only difference is that the node is represented as a list [isLeaf, val].

If the value of isLeaf or val is True we represent it as 1 in the list [isLeaf, val] and if the value of isLeaf or val is False we represent it as 0.

```cpp
class Solution {
public:
    Node* dfs(vector<vector<int>>&grid,int n,int r,int c){
        if(n==1)
        return new Node(grid[r][c]==1,1);
        int mid=n/2;
        Node*topLeft=dfs(grid,mid,r,c);
        Node*topRight=dfs(grid,mid,r,c+mid);
        Node*bottomLeft=dfs(grid,mid,r+mid,c);
        Node*bottomRight=dfs(grid,mid,r+mid,c+mid);
        if(topLeft->isLeaf && topRight->isLeaf && bottomLeft->isLeaf &&
        bottomRight->isLeaf && topLeft->val==topRight->val &&
        topLeft->val==bottomLeft->val && topLeft->val==bottomRight->val)
        {
            return new Node(topLeft->val,1);
        }
        return new Node(0,0,topLeft,topRight,bottomLeft,bottomRight);
    }
    Node* construct(vector<vector<int>>& grid) {
        return dfs(grid,grid.size(),0,0);
    }
};
```

## 16 Count Good Nodes in Binary Tree

Given a binary tree root, a node X in the tree is named good if in the path from root to X there are no nodes with a value greater than X.

Return the number of good nodes in the binary tree.

```cpp
class Solution {
    int cnt=0;
public:
    void solve(TreeNode* root,int maxo){
        if(!root)
        return ;
        if(root->val>=maxo){
            cnt++;
            maxo=root->val;
        }
        solve(root->left,maxo);
        solve(root->right,maxo);
    }
    int goodNodes(TreeNode* root) {
        solve(root,root->val);
        return cnt;
    }
};
```

## 17 Validate Binary Search Tree

Given the root of a binary tree, determine if it is a valid binary search tree (BST).

A valid BST is defined as follows:

The left subtree of a node contains only nodes with keys strictly less than the node's key.
The right subtree of a node contains only nodes with keys strictly greater than the node's key.
Both the left and right subtrees must also be binary search trees.

```cpp
class Solution {
    vector<int>v;
public:
    void solve(TreeNode*root){
        if(!root)
        return ;
        solve(root->left);
        v.push_back(root->val);
        solve(root->right);
    }
    bool isValidBST(TreeNode* root) {
        solve(root);
        for(int i=1;i<v.size();i++){
            if(v[i]<=v[i-1])
            return 0;
        }
        return 1;
    }
};
```

## 18 Kth Smallest Element in a BST

Given the root of a binary search tree, and an integer k, return the kth smallest value (1-indexed) of all the values of the nodes in the tree.

```cpp
class Solution {
    int ele;
public:
    bool solve(TreeNode*root,int &cnt,int k){
        if(!root)
        return 0;
        if(solve(root->left,cnt,k))
        return 1;
        if(cnt==k)
        {
            ele=root->val;
            return 1;
        }
        cnt++;
        if(solve(root->right,cnt,k))
        return 1;
        return 0;
    }
    int kthSmallest(TreeNode* root, int k) {
        int cnt=1;
        solve(root,cnt,k);
        return ele;
    }
};
```

## 19 Construct Binary Tree from Preorder and Inorder Traversal

Given two integer arrays preorder and inorder where preorder is the preorder traversal of a binary tree and inorder is the inorder traversal of the same tree, construct and return the binary tree.

```cpp
class Solution {
    unordered_map<int,int>mp;
public:
    TreeNode* solve(vector<int>& preorder,int &preidx,int l,int r){
        if(l>r)
        return NULL;
        int rootval=preorder[preidx++];
        int mid=mp[rootval];
        TreeNode*root=new TreeNode(rootval);
        root->left=solve(preorder,preidx,l,mid-1);
        root->right=solve(preorder,preidx,mid+1,r);
        return root;
    }
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        for(int i=0;i<inorder.size();i++)
        mp[inorder[i]]=i;
        int preidx=0;
        return solve(preorder,preidx,0,preorder.size()-1);
    }
};
```

## 20 House Robber III

The thief has found himself a new place for his thievery again. There is only one entrance to this area, called root.

Besides the root, each house has one and only one parent house. After a tour, the smart thief realized that all houses in this place form a binary tree. It will automatically contact the police if two directly-linked houses were broken into on the same night.

Given the root of the binary tree, return the maximum amount of money the thief can rob without alerting the police.

```cpp
class Solution {
    int maxsum=0;
    unordered_map<TreeNode*,unordered_map<bool,int>>mp;
public:
    int find(TreeNode* root,bool take){
        if(!root)
        return 0;
        if(mp.count(root) && mp[root].count(take))
        return mp[root][take];
        else
        return mp[root][take]=solve(root,take);
    }
    int solve(TreeNode* root,bool take){
        if(!root)
            return 0;
        if(take){
            int s1=find(root->left,!take);
            int s2=find(root->right,!take);
            return s1+s2+root->val;
        }
        else{
            int s1=find(root->left,!take);
            s1=max(s1,find(root->left,take));
            int s2=find(root->right,!take);
            s2=max(s2,find(root->right,take));
            return s1+s2;
        }
    }
    int rob(TreeNode* root) {
        int sum1=find(root,0);
        int sum2=find(root,1);
        maxsum=max(sum1,sum2);
        return maxsum;
    }
};
```

## 21 Delete Leaves With a Given Value

Given a binary tree root and an integer target, delete all the leaf nodes with value target.

Note that once you delete a leaf node with value target, if its parent node becomes a leaf node and has the value target, it should also be deleted (you need to continue doing that until you cannot).

```cpp
class Solution {
public:
    TreeNode* solve(TreeNode*root,int target){
        if(!root)
        return NULL;
        root->left=solve(root->left,target);
        root->right=solve(root->right,target);
        if(!root->left && !root->right && root->val==target)
            return NULL;
        return root;
    }
    TreeNode* removeLeafNodes(TreeNode* root, int target) {
        return solve(root,target);
    }
};
```

## 22 Binary Tree Maximum Path Sum

A path in a binary tree is a sequence of nodes where each pair of adjacent nodes in the sequence has an edge connecting them. A node can only appear in the sequence at most once. Note that the path does not need to pass through the root.

The path sum of a path is the sum of the node's values in the path.

Given the root of a binary tree, return the maximum path sum of any non-empty path.

```cpp
class Solution {
    int maxsum=INT_MIN;
public:
    int solve(TreeNode* root){
        if(!root)
        return 0;
        int leftsum=max(0,solve(root->left)),rightsum=max(0,solve(root->right));
        maxsum=max(maxsum,root->val+leftsum+rightsum);
        return max(leftsum,rightsum)+root->val;
    }
    int maxPathSum(TreeNode* root) {
        solve(root);
        return maxsum;
    }
};
```

## 23 Serialize and Deserialize Binary Tree

Serialization is the process of converting a data structure or object into a sequence of bits so that it can be stored in a file or memory buffer, or transmitted across a network connection link to be reconstructed later in the same or another computer environment.

Design an algorithm to serialize and deserialize a binary tree. There is no restriction on how your serialization/deserialization algorithm should work. You just need to ensure that a binary tree can be serialized to a string and this string can be deserialized to the original tree structure.

Clarification: The input/output format is the same as how LeetCode serializes a binary tree. You do not necessarily need to follow this format, so please be creative and come up with different approaches yourself.

```cpp
class Codec {
public:

    // Encodes a tree to a single string.
    string serialize(TreeNode* root) {
        if(!root)
        return "#";
        string res;
        queue<TreeNode*>q;
        q.push(root);
        while(!q.empty()){
            TreeNode* node=q.front();
            q.pop();
            if(node->val!=-9999)
            res+=to_string(node->val)+"\t";
            else
            {res+="#\t";continue;}
            if(node->left)
            q.push(node->left);
            else
            q.push(new TreeNode(-9999));
            if(node->right)
            q.push(node->right);
            else
            q.push(new TreeNode(-9999));
        }
        return res;
    }

    // Decodes your encoded data to tree.
    TreeNode* deserialize(string data) {
        if(data=="#")
        return NULL;
        int i=0;
        string rootnum=data.substr(i,data.find('\t'));
        TreeNode*root=new TreeNode(stoi(rootnum));
        i+=rootnum.size()+1;
        queue<TreeNode*>q;
        q.push(root);
        while(!q.empty()){
            TreeNode* node=q.front();
            q.pop();
            string temp=data.substr(i,data.substr(i).find('\t'));
            if(temp=="#")
            i+=temp.size()+1;
            else{
                node->left=new TreeNode(stoi(temp));
                q.push(node->left);
                i+=temp.size()+1;
            }
            temp=data.substr(i,data.substr(i).find('\t'));
            if(temp=="#")
            i+=temp.size()+1;
            else{
                node->right=new TreeNode(stoi(temp));
                q.push(node->right);
                i+=temp.size()+1;
            }
        }
        return root;
    }
};
```
