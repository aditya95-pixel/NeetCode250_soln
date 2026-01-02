## 1 Unique Paths

There is a robot on an m x n grid. The robot is initially located at the top-left corner (i.e., grid[0][0]). The robot tries to move to the bottom-right corner (i.e., grid[m - 1][n - 1]). The robot can only move either down or right at any point in time.

Given the two integers m and n, return the number of possible unique paths that the robot can take to reach the bottom-right corner.

The test cases are generated so that the answer will be less than or equal to 2 * 109.

```cpp
class Solution {
public:
    int uniquePaths(int m, int n) {
        vector<vector<int>>dp(m,vector<int>(n,0));
        dp[0][0]=1;
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(i-1>=0)
                dp[i][j]+=dp[i-1][j];
                if(j-1>=0)
                dp[i][j]+=dp[i][j-1];
            }
        }
        return dp[m-1][n-1];
    }
};
```

## 2 Unique Paths II

You are given an m x n integer array grid. There is a robot initially located at the top-left corner (i.e., grid[0][0]). The robot tries to move to the bottom-right corner (i.e., grid[m - 1][n - 1]). The robot can only move either down or right at any point in time.

An obstacle and space are marked as 1 or 0 respectively in grid. A path that the robot takes cannot include any square that is an obstacle.

Return the number of possible unique paths that the robot can take to reach the bottom-right corner.

The testcases are generated so that the answer will be less than or equal to 2 * 109.

```cpp
class Solution {
public:
    int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
        if(obstacleGrid[obstacleGrid.size()-1][obstacleGrid[0].size()-1]==1)
        return 0;
        vector<vector<int>>dp(obstacleGrid.size(),vector<int>(obstacleGrid[0].size(),0));
        dp[0][0]=1;
        for(int i=0;i<obstacleGrid.size();i++){
            for(int j=0;j<obstacleGrid[0].size();j++){
                if(obstacleGrid[i][j]==1)
                continue;
                if(i-1>=0 && obstacleGrid[i-1][j]==0)
                dp[i][j]+=dp[i-1][j];
                if(j-1>=0 && obstacleGrid[i][j-1]==0)
                dp[i][j]+=dp[i][j-1];
            }
        }
        return dp[obstacleGrid.size()-1][obstacleGrid[0].size()-1];
    }
};
```

## 3 Minimum Path Sum

Given a m x n grid filled with non-negative numbers, find a path from top left to bottom right, which minimizes the sum of all numbers along its path.

Note: You can only move either down or right at any point in time.

```cpp
class Solution {
public:
    int minPathSum(vector<vector<int>>& grid) {
        vector<vector<int>>dp(grid.size(),vector<int>(grid[0].size(),INT_MAX));
        dp[0][0]=grid[0][0];
        for(int i=0;i<grid.size();i++){
            for(int j=0;j<grid[0].size();j++){
                if(i-1>=0)
                dp[i][j]=dp[i-1][j]+grid[i][j];
                if(j-1>=0)
                dp[i][j]=min(dp[i][j],dp[i][j-1]+grid[i][j]);
            }
        }
        return dp[grid.size()-1][grid[0].size()-1];
    }
};
```

## 4 Longest Common Subsequence

Given two strings text1 and text2, return the length of their longest common subsequence. If there is no common subsequence, return 0.

A subsequence of a string is a new string generated from the original string with some characters (can be none) deleted without changing the relative order of the remaining characters.

For example, "ace" is a subsequence of "abcde".
A common subsequence of two strings is a subsequence that is common to both strings.

```cpp
class Solution {
public:
    int longestCommonSubsequence(string text1, string text2) {
        vector<vector<int>>dp(text1.size()+1,vector<int>(text2.size()+1,0));
        for(int i=1;i<=text1.size();i++){
            for(int j=1;j<=text2.size();j++){
                if(text1[i-1]==text2[j-1])
                dp[i][j]=1+dp[i-1][j-1];
                else
                dp[i][j]=max(dp[i-1][j],dp[i][j-1]);
            }
        }
        return dp[text1.size()][text2.size()];
    }
};
```

## 5 Last Stone Weight II

You are given an array of integers stones where stones[i] is the weight of the ith stone.

We are playing a game with the stones. On each turn, we choose any two stones and smash them together. Suppose the stones have weights x and y with x <= y. The result of this smash is:

If x == y, both stones are destroyed, and
If x != y, the stone of weight x is destroyed, and the stone of weight y has new weight y - x.
At the end of the game, there is at most one stone left.

Return the smallest possible weight of the left stone. If there are no stones left, return 0.

```cpp
class Solution {
public:
    int lastStoneWeightII(vector<int>& stones) {
        int sum=accumulate(stones.begin(),stones.end(),0);
        int target=sum/2;
        vector<vector<int>>dp(stones.size()+1,vector<int>(target+1,0));
        for(int i=1;i<=stones.size();i++){
            for(int j=0;j<=target;j++){
                dp[i][j]=dp[i-1][j];
                if(j-stones[i-1]>=0)
                dp[i][j]=max(dp[i][j],dp[i-1][j-stones[i-1]]+stones[i-1]);
            }
        }
        return sum-2*dp[stones.size()][target];
    }
};
```

## 6 Best Time to Buy and Sell Stock with Cooldown

You are given an array prices where prices[i] is the price of a given stock on the ith day.

Find the maximum profit you can achieve. You may complete as many transactions as you like (i.e., buy one and sell one share of the stock multiple times) with the following restrictions:

After you sell your stock, you cannot buy stock on the next day (i.e., cooldown one day).
Note: You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        vector<vector<int>>dp(prices.size(),vector<int>(2,0));
        for(int i=prices.size()-1;i>=0;i--){
            //sell
            dp[i][0]=max((i+1<prices.size())?dp[i+1][0]:prices[i],(i+2<prices.size())?(dp[i+2][1]+prices[i]):prices[i]);
            //buy
            dp[i][1]=max((i+1<prices.size())?dp[i+1][1]:0,(i+1<prices.size())?(dp[i+1][0]-prices[i]):0);
        }
        return dp[0][1];
    }
};
```

## 7 Coin Change II

You are given an integer array coins representing coins of different denominations and an integer amount representing a total amount of money.

Return the number of combinations that make up that amount. If that amount of money cannot be made up by any combination of the coins, return 0.

You may assume that you have an infinite number of each kind of coin.

The answer is guaranteed to fit into a signed 32-bit integer.

```cpp
class Solution {
public:
    int change(int amount, vector<int>& coins) {
        vector<uint>dp(amount+1,0);
        dp[0]=1;
        for(int i=0;i<coins.size();i++){
            for(int j=coins[i];j<=amount;j++)
            dp[j]+=dp[j-coins[i]];
        }
        return dp[amount];
    }
};
```

## 8 Target Sum

You are given an integer array nums and an integer target.

You want to build an expression out of nums by adding one of the symbols '+' and '-' before each integer in nums and then concatenate all the integers.

For example, if nums = [2, 1], you can add a '+' before 2 and a '-' before 1 and concatenate them to build the expression "+2-1".
Return the number of different expressions that you can build, which evaluates to target.

 ```cpp
class Solution {
public:
    int findTargetSumWays(vector<int>& nums, int target) {
        unordered_map<int,unordered_map<int,int>>dp;
        dp[0][nums[0]]++;
        dp[0][-nums[0]]++;
        for(int i=1;i<nums.size();i++){
            for(auto &item:dp[i-1]){
                int val=item.first,cnt=item.second;
                dp[i][val+nums[i]]+=cnt;
                dp[i][val-nums[i]]+=cnt;
            }
        }
        return dp[nums.size()-1][target];
    }
};
```

## 9 Interleaving String

Given strings s1, s2, and s3, find whether s3 is formed by an interleaving of s1 and s2.

An interleaving of two strings s and t is a configuration where s and t are divided into n and m substrings respectively, such that:

s = s1 + s2 + ... + sn
t = t1 + t2 + ... + tm
|n - m| <= 1
The interleaving is s1 + t1 + s2 + t2 + s3 + t3 + ... or t1 + s1 + t2 + s2 + t3 + s3 + ...
Note: a + b is the concatenation of strings a and b.

```cpp
class Solution {
public:
    bool isInterleave(string s1, string s2, string s3) {
        if(s1.size()+s2.size()!=s3.size())
        return 0;
        vector<vector<int>>dp(s1.size()+1,vector<int>(s2.size()+1,0));
        dp[0][0]=1;
        for(int i=1;i<=s1.size();i++)
        {
            if(!dp[i-1][0])
            break;
            else if(s3[i-1]==s1[i-1])
            dp[i][0]=dp[i-1][0];
        }
        for(int j=1;j<=s2.size();j++)
        {
            if(!dp[0][j-1])
            break;
            else if(s3[j-1]==s2[j-1])
            dp[0][j]=dp[0][j-1];
        }
        for(int i=1;i<=s1.size();i++){
            for(int j=1;j<=s2.size();j++){
                int k=i+j;
                if(dp[i-1][j] && s3[k-1]==s1[i-1])
                dp[i][j]=dp[i-1][j];
                if(dp[i][j-1] && s3[k-1]==s2[j-1])
                dp[i][j]=dp[i][j-1];
            }
        }
        return dp[s1.size()][s2.size()];
    }
};
```

## 10 Stone Game

Alice and Bob play a game with piles of stones. There are an even number of piles arranged in a row, and each pile has a positive integer number of stones piles[i].

The objective of the game is to end with the most stones. The total number of stones across all the piles is odd, so there are no ties.

Alice and Bob take turns, with Alice starting first. Each turn, a player takes the entire pile of stones either from the beginning or from the end of the row. This continues until there are no more piles left, at which point the person with the most stones wins.

Assuming Alice and Bob play optimally, return true if Alice wins the game, or false if Bob wins.

```cpp
class Solution {
public:
    int solve(vector<int>& piles,int l,int r,vector<vector<int>>&dp,int sum){
        if(l+1>r)
        return 0;
        if(l+1==r)
        return max(piles[l],piles[r]);
        if(dp[l][r]!=-1)
        return dp[l][r];
        int score1=solve(piles,l+1,r,dp,sum-piles[l]);
        int score2=solve(piles,l,r-1,dp,sum-piles[r]);
        return dp[l][r]=sum-min(score1,score2);
    }
    bool stoneGame(vector<int>& piles) {
        vector<vector<int>>dp(piles.size(),vector<int>(piles.size(),-1));
        int sum=accumulate(piles.begin(),piles.end(),0);
        return (solve(piles,0,piles.size()-1,dp,sum)>(double)sum/2);
    }
};
```

## 11 Stone Game II

Alice and Bob continue their games with piles of stones. There are a number of piles arranged in a row, and each pile has a positive integer number of stones piles[i]. The objective of the game is to end with the most stones.

Alice and Bob take turns, with Alice starting first.

On each player's turn, that player can take all the stones in the first X remaining piles, where 1 <= X <= 2M. Then, we set M = max(M, X). Initially, M = 1.

The game continues until all the stones have been taken.

Assuming Alice and Bob play optimally, return the maximum number of stones Alice can get.

```cpp
class Solution {
    vector<vector<int>>dp;
public:
    int solve(int sum,vector<int>& piles,int m,int idx){
        if(idx==piles.size())
        return 0;
        if(dp[m][idx]!=-1)
        return dp[m][idx];
        int mino=INT_MAX,sum1=sum;
        for(int i=idx;i<min(idx+2*m,(int)piles.size());i++){
            sum1-=piles[i];
            mino=min(mino,solve(sum1,piles,max(m,i-idx+1),i+1));
        }
        return dp[m][idx]=sum-mino;
    }
    int stoneGameII(vector<int>& piles) {
        dp.resize(piles.size()+1,vector<int>(piles.size(),-1));
        int sum=accumulate(piles.begin(),piles.end(),0);
        return solve(sum,piles,1,0);
    }
};
```

## 12 Longest Increasing Path in a Matrix

Given an m x n integers matrix, return the length of the longest increasing path in matrix.

From each cell, you can either move in four directions: left, right, up, or down. You may not move diagonally or move outside the boundary (i.e., wrap-around is not allowed).

```cpp
class Solution {
    vector<vector<int>>dp;
public:
    int solve(int i,int j,vector<vector<int>>& matrix){
        if(dp[i][j]!=-1)
        return dp[i][j];
        int maxlen=1;
        if(i-1>=0 && matrix[i-1][j]<matrix[i][j])
        maxlen=max(maxlen,1+solve(i-1,j,matrix));
        if(j-1>=0 && matrix[i][j-1]<matrix[i][j])
        maxlen=max(maxlen,1+solve(i,j-1,matrix));
        if(i+1<matrix.size() && matrix[i+1][j]<matrix[i][j])
        maxlen=max(maxlen,1+solve(i+1,j,matrix));
        if(j+1<matrix[0].size() && matrix[i][j+1]<matrix[i][j])
        maxlen=max(maxlen,1+solve(i,j+1,matrix));
        return dp[i][j]=maxlen;
    }
    int longestIncreasingPath(vector<vector<int>>& matrix) {
        int maxlen=1;
        dp.resize(matrix.size(),vector<int>(matrix[0].size(),-1));
        for(int i=0;i<matrix.size();i++){
            for(int j=0;j<matrix[0].size();j++)
                maxlen=max(maxlen,solve(i,j,matrix));
        }
        return maxlen;
    }
};
```

## 13 Distinct Subsequences

Given two strings s and t, return the number of distinct subsequences of s which equals t.

The test cases are generated so that the answer fits on a 32-bit signed integer.

```cpp
class Solution {
public:
    int numDistinct(string s, string t) {
        vector<vector<uint>>dp(s.size()+1,vector<uint>(t.size()+1,0));
        for(int i=t.size();i<=s.size();i++)
        dp[i][t.size()]=1;
        for(int i=s.size()-1;i>=0;i--){
            for(int j=t.size()-1;j>=0;j--){
                dp[i][j]=dp[i+1][j];
                if(s[i]==t[j])
                dp[i][j]+=dp[i+1][j+1];
            }
        }
        return dp[0][0];
    }
};
```
