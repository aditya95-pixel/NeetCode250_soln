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

