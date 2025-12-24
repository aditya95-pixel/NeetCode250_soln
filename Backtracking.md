## 1 Sum of All Subset XOR Totals

The XOR total of an array is defined as the bitwise XOR of all its elements, or 0 if the array is empty.

For example, the XOR total of the array [2,5,6] is 2 XOR 5 XOR 6 = 1.
Given an array nums, return the sum of all XOR totals for every subset of nums. 

Note: Subsets with the same elements should be counted multiple times.

An array a is a subset of an array b if a can be obtained from b by deleting some (possibly zero) elements of b.

```cpp
class Solution {
public:
    void solve(vector<int>&nums,int idx,int &sum,int curr){
        if(idx==nums.size())
        {
            sum+=curr;
            return ;
        }
        solve(nums,idx+1,sum,curr^nums[idx]);
        solve(nums,idx+1,sum,curr);
    }
    int subsetXORSum(vector<int>& nums) {
        int sum=0;
        solve(nums,0,sum,0);
        return sum;
    }
};
```

## 2 Subsets

Given an integer array nums of unique elements, return all possible subsets (the power set).

The solution set must not contain duplicate subsets. Return the solution in any order.

```cpp
class Solution {
public:
    void solve(vector<int>&nums,int idx,vector<vector<int>>&res,vector<int>&temp){
        if(idx==nums.size()){
            res.push_back(temp);
            return ;
        }
        solve(nums,idx+1,res,temp);
        temp.push_back(nums[idx]);
        solve(nums,idx+1,res,temp);
        temp.pop_back();
    }
    vector<vector<int>> subsets(vector<int>& nums) {
        vector<vector<int>>res;
        vector<int>temp;
        solve(nums,0,res,temp);
        return res;
    }
};
```

## 3 Combination Sum

Given an array of distinct integers candidates and a target integer target, return a list of all unique combinations of candidates where the chosen numbers sum to target. You may return the combinations in any order.

The same number may be chosen from candidates an unlimited number of times. Two combinations are unique if the frequency of at least one of the chosen numbers is different.

The test cases are generated such that the number of unique combinations that sum up to target is less than 150 combinations for the given input.

```cpp
class Solution {
public:
    void solve(vector<int>& nums, int target,int sum,vector<vector<int>>&res,vector<int>&temp,int idx){
        if(sum>target)
        return; 
        else if(sum==target)
        {
            res.push_back(temp);
            return ;
        }
        for(int i=idx;i<nums.size();i++){
            temp.push_back(nums[i]);
            sum+=nums[i];
            solve(nums,target,sum,res,temp,i);
            sum-=nums[i];
            temp.pop_back();
        }
    }
    vector<vector<int>> combinationSum(vector<int>& nums, int target) {
        vector<vector<int>>res;
        vector<int>temp;
        solve(nums,target,0,res,temp,0);
        return res;
    }
};
```

## 4 Combination Sum II

Given a collection of candidate numbers (candidates) and a target number (target), find all unique combinations in candidates where the candidate numbers sum to target.

Each number in candidates may only be used once in the combination.

Note: The solution set must not contain duplicate combinations.

```cpp
class Solution {
public:
    void solve(vector<int>& nums, int target,int sum,vector<vector<int>>&res,vector<int>&temp,int idx){
        if(sum>target)
        return; 
        else if(sum==target)
        {
            res.push_back(temp);
            return ;
        }
        for(int i=idx;i<nums.size();i++){
            if(i>idx && nums[i]==nums[i-1])
            continue;
            temp.push_back(nums[i]);
            sum+=nums[i];
            solve(nums,target,sum,res,temp,i+1);
            sum-=nums[i];
            temp.pop_back();
        }
    }
    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
        sort(candidates.begin(),candidates.end());
        vector<vector<int>>res;
        vector<int>temp;
        solve(candidates,target,0,res,temp,0);
        return res;
    }
};
```

## 5 Combinations

Given two integers n and k, return all possible combinations of k numbers chosen from the range [1, n].

You may return the answer in any order.

```cpp
class Solution {
public:
    void solve(int n,int k,vector<vector<int>>&res,vector<int>&temp,int curr){
        if(curr>n && k>0)
        return ;
        if(k==0){
            res.push_back(temp);
            return ;
        }
        solve(n,k,res,temp,curr+1);
        temp.push_back(curr);
        solve(n,k-1,res,temp,curr+1);
        temp.pop_back();
    }
    vector<vector<int>> combine(int n, int k) {
        vector<vector<int>>res;
        vector<int>temp;
        solve(n,k,res,temp,1);
        return res;
    }
};
```

## 6 Permutations

Given an array nums of distinct integers, return all the possible permutations. You can return the answer in any order.

```cpp
class Solution {
public:
    void solve(set<int>s,vector<int>&nums,vector<vector<int>>&res,vector<int>&temp){
        if(s.size()==0)
        {
            res.push_back(temp);
            return ;
        }
        for(auto &ele:nums){
            if(s.count(ele)){
                temp.push_back(ele);
                s.erase(ele);
                solve(s,nums,res,temp);
                s.insert(ele);
                temp.pop_back();
            }
        }
    }
    vector<vector<int>> permute(vector<int>& nums) {
        set<int>s(nums.begin(),nums.end());
        vector<vector<int>>res;
        vector<int>temp;
        solve(s,nums,res,temp);
        return res;
    }
};
```

## 7 Subsets II

Given an integer array nums that may contain duplicates, return all possible subsets (the power set).

The solution set must not contain duplicate subsets. Return the solution in any order.

```cpp
class Solution{
public:
    void solve(vector<int>&nums,vector<vector<int>>&res,vector<int>&temp,int idx,int skip){
        if(idx==nums.size()){
            res.push_back(temp);
            return ;
        }
        solve(nums,res,temp,idx+1,nums[idx]);
        if(skip==nums[idx])
        return ;
        temp.push_back(nums[idx]);
        solve(nums,res,temp,idx+1,INT_MIN);
        temp.pop_back();
    }
    vector<vector<int>> subsetsWithDup(vector<int>& nums) {
        sort(nums.begin(),nums.end());
        vector<vector<int>>res;
        vector<int>temp;
        solve(nums,res,temp,0,INT_MIN);
        return res;
    }
};
```

## 8 Permutations II

Given a collection of numbers, nums, that might contain duplicates, return all possible unique permutations in any order.

```cpp
class Solution {
public:
    void solve(multiset<int>s,vector<int>&nums,vector<vector<int>>&res,vector<int>&temp){
        if(s.size()==0)
        {
            res.push_back(temp);
            return ;
        }
        set<int>used;
        for(auto &ele:nums){
            if(!used.count(ele) && s.count(ele)){
                temp.push_back(ele);
                s.erase(s.lower_bound(ele));
                solve(s,nums,res,temp);
                s.insert(ele);
                temp.pop_back();
            }
            used.insert(ele);
        }
    }
    vector<vector<int>> permuteUnique(vector<int>& nums) {
        multiset<int>s(nums.begin(),nums.end());
        vector<vector<int>>res;
        vector<int>temp;
        solve(s,nums,res,temp);
        return res;
    }
};
```

## 9 Generate Parentheses

Given n pairs of parentheses, write a function to generate all combinations of well-formed parentheses.

```cpp
class Solution {
public:
    void solve(vector<string>&res,string&temp,int op,int cp,int n){
        if(op==n && cp==n)
        {
            res.push_back(temp);
            return ;
        }
        if(op<n)
        {
            temp+='(';
            solve(res,temp,op+1,cp,n);
            temp.pop_back();
        }
        if(op>cp)
        {
            temp+=')';
            solve(res,temp,op,cp+1,n);
            temp.pop_back();
        }
    }
    vector<string> generateParenthesis(int n) {
        vector<string>res;
        string temp;
        solve(res,temp,0,0,n);
        return res;
    }
};
```

## 10 Word Search

Given an m x n grid of characters board and a string word, return true if word exists in the grid.

The word can be constructed from letters of sequentially adjacent cells, where adjacent cells are horizontally or vertically neighboring. The same letter cell may not be used more than once.

```cpp
class Solution {
public:
    bool solve(vector<vector<char>>& board,string &word,int i,int j,string &temp,int idx){
        if(i<0 || j<0 || i>=board.size() || j>=board[0].size())
        return 0;
        if(board[i][j]!=word[idx])
        return 0;
        temp+=board[i][j];
        idx++;
        char c=board[i][j];
        board[i][j]='.';
        if(idx==word.size())
        return 1;
        if(solve(board,word,i+1,j,temp,idx))
        return 1;
        if(solve(board,word,i-1,j,temp,idx))
        return 1;
        if(solve(board,word,i,j+1,temp,idx))
        return 1;
        if(solve(board,word,i,j-1,temp,idx))
        return 1;
        temp.pop_back();
        board[i][j]=c;
        return 0;
    }
    bool exist(vector<vector<char>>& board, string word) {
        for(int i=0;i<board.size();i++){
            for(int j=0;j<board[0].size();j++){
                if(board[i][j]==word[0]){
                    string temp;
                    if(solve(board,word,i,j,temp,0))
                    return 1;
                }
            }
        }
        return 0;
    }
};
```

## 11 Palindrome Partitioning

Given a string s, partition s such that every substring of the partition is a palindrome. Return all possible palindrome partitioning of s.

```cpp
class Solution {
public:
    bool ispalin(string str){
        for(int i=0;i<str.size()/2;i++){
            if(str[i]!=str[str.size()-i-1])
            return 0;
        }
        return 1;
    }
    void solve(string &s,vector<vector<string>>&res,vector<string>&temp,int idx){
        if(idx==s.size()){
            res.push_back(temp);
            return ;
        }
        for(int i=idx;i<s.size();i++){
            string str=s.substr(idx,i-idx+1);
            if(ispalin(str)){
                temp.push_back(str);
                solve(s,res,temp,i+1);
                temp.pop_back();
            }
        }
    }
    vector<vector<string>> partition(string s) {
        vector<vector<string>>res;
        vector<string>temp;
        solve(s,res,temp,0);
        return res;
    }
};
```

## 12 Letter Combinations of a Phone Number

Given a string containing digits from 2-9 inclusive, return all possible letter combinations that the number could represent. Return the answer in any order.

A mapping of digits to letters (just like on the telephone buttons) is given below. Note that 1 does not map to any letters.

```cpp
class Solution {
public:
    void solve(string &digits,map<char,vector<char>>&mp,vector<string>&res,string &temp){
        if(temp.size()==digits.size()){
            res.push_back(temp);
            return ;
        }
        int i=temp.size();
        for(auto &c:mp[digits[i]]){
            temp+=c;
            solve(digits,mp,res,temp);
            temp.pop_back();
        }
    }
    vector<string> letterCombinations(string digits) {
        vector<string>res;
        if(digits.size()==0)
        return res;
        map<char,vector<char>>mp={{'2',{'a','b','c'}},{'3',{'d','e','f'}},
        {'4',{'g','h','i'}},{'5',{'j','k','l'}},{'6',{'m','n','o'}},
        {'7',{'p','q','r','s'}},{'8',{'t','u','v'}},{'9',{'w','x','y','z'}}};
        string temp;
        solve(digits,mp,res,temp);
        return res;
    }
};
```

## 13 Matchsticks to Square

You are given an integer array matchsticks where matchsticks[i] is the length of the ith matchstick. You want to use all the matchsticks to make one square. You should not break any stick, but you can link them up, and each matchstick must be used exactly one time.

Return true if you can make this square and false otherwise.

```cpp
class Solution {
public:
    bool solve(vector<int>& nums, int k,int curr,int target,int idx,vector<bool>&used){
        if(k==0)
        return 1;
        if(curr==target)
        return solve(nums,k-1,0,target,0,used);
        for(int i=idx;i<nums.size();i++){
            if(used[i] || curr+nums[i]>target)
            continue;
            used[i]=1;
            if(solve(nums,k,curr+nums[i],target,i+1,used))
            return 1;
            used[i]=0;
            if(curr==0)
            return 0;
        }
        return 0;
    }
    bool canPartitionKSubsets(vector<int>& nums, int k) {
        int sum=accumulate(nums.begin(),nums.end(),0);
        if(sum%k!=0)
        return 0;
        int target=sum/k;
        vector<bool>used(nums.size(),0);
        sort(nums.rbegin(),nums.rend());
        return solve(nums,k,0,target,0,used);
    }
    bool makesquare(vector<int>& matchsticks) {
        return canPartitionKSubsets(matchsticks,4);
    }
};
```

## 14 Partition to K Equal Sum Subsets

Given an integer array nums and an integer k, return true if it is possible to divide this array into k non-empty subsets whose sums are all equal.

```cpp
class Solution {
public:
    bool solve(vector<int>& nums, int k,int curr,int target,int idx,vector<bool>&used){
        if(k==0)
        return 1;
        if(curr==target)
        return solve(nums,k-1,0,target,0,used);
        for(int i=idx;i<nums.size();i++){
            if(used[i] || curr+nums[i]>target)
            continue;
            used[i]=1;
            if(solve(nums,k,curr+nums[i],target,i+1,used))
            return 1;
            used[i]=0;
            if(curr==0)
            return 0;
        }
        return 0;
    }
    bool canPartitionKSubsets(vector<int>& nums, int k) {
        int sum=accumulate(nums.begin(),nums.end(),0);
        if(sum%k!=0)
        return 0;
        int target=sum/k;
        vector<bool>used(nums.size(),0);
        sort(nums.rbegin(),nums.rend());
        return solve(nums,k,0,target,0,used);
    }
};
```

## 15 N-Queens

The n-queens puzzle is the problem of placing n queens on an n x n chessboard such that no two queens attack each other.

Given an integer n, return all distinct solutions to the n-queens puzzle. You may return the answer in any order.

Each solution contains a distinct board configuration of the n-queens' placement, where 'Q' and '.' both indicate a queen and an empty space, respectively.

```cpp
class Solution {
public:
    bool check(vector<string>&board,int row,int col){
        for(int i=0;i<row;i++){
            if(board[i][col]=='Q')
            return 0;
        }
        int i1=row-1,j1=col-1;
        while(i1>=0 && j1>=0){
            if(board[i1][j1]=='Q')
            return 0;
            i1--;
            j1--;
        }
        i1=row-1;
        j1=col+1;
        while(i1>=0 && j1<board.size()){
            if(board[i1][j1]=='Q')
            return 0;
            i1--;
            j1++;
        }
        return 1;
    }
    void solve(vector<vector<string>>&res,vector<string>&board,int i){
        if(i==board.size()){
            res.push_back(board);
            return ;
        }
        for(int j=0;j<board.size();j++){
            if(check(board,i,j))
            {
                board[i][j]='Q';
                solve(res,board,i+1);
                board[i][j]='.';
            }
        }
    }
    vector<vector<string>> solveNQueens(int n) {
        vector<vector<string>>res;
        vector<string>board(n,string(n,'.'));
        solve(res,board,0);
        return res;
    }
};
```

## 16 N-Queens II

The n-queens puzzle is the problem of placing n queens on an n x n chessboard such that no two queens attack each other.

Given an integer n, return the number of distinct solutions to the n-queens puzzle.

```cpp
class Solution {
    int cnt=0;
public:
    bool check(vector<string>&board,int row,int col){
        for(int i=0;i<row;i++){
            if(board[i][col]=='Q')
            return 0;
        }
        int i1=row-1,j1=col-1;
        while(i1>=0 && j1>=0){
            if(board[i1][j1]=='Q')
            return 0;
            i1--;
            j1--;
        }
        i1=row-1;
        j1=col+1;
        while(i1>=0 && j1<board.size()){
            if(board[i1][j1]=='Q')
            return 0;
            i1--;
            j1++;
        }
        return 1;
    }
    void solve(vector<string>&board,int i){
        if(i==board.size()){
            cnt++;
            return ;
        }
        for(int j=0;j<board.size();j++){
            if(check(board,i,j))
            {
                board[i][j]='Q';
                solve(board,i+1);
                board[i][j]='.';
            }
        }
    }
    int totalNQueens(int n) {
        vector<string>board(n,string(n,'.'));
        solve(board,0);
        return cnt;
    }
};
```

## 17 Word Break II

Given a string s and a dictionary of strings wordDict, add spaces in s to construct a sentence where each word is a valid dictionary word. Return all such possible sentences in any order.

Note that the same word in the dictionary may be reused multiple times in the segmentation.

```cpp
class Solution {
public:
    void solve(string s,vector<string>&res,string temp,set<string>&seto,int end){
        if(end<0){
            res.push_back(temp);
            return ;
        }
        for(int i=end;i>=0;i--){
            string str=s.substr(i,end-i+1);
            if(seto.count(str))
                solve(s,res,str+((temp=="")?"":" ")+temp,seto,i-1);
        }
    }
    vector<string> wordBreak(string s, vector<string>& wordDict) {
        set<string>seto(wordDict.begin(),wordDict.end());
        vector<string>res;
        string temp;
        solve(s,res,temp,seto,s.size()-1);
        return res;
    }
};
```
