## 1 Climbing Stairs

You are climbing a staircase. It takes n steps to reach the top.

Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?

```cpp
class Solution {
public:
    int climbStairs(int n) {
        vector<int>dp(n+1,0);
        dp[0]=1;
        for(int i=1;i<=n;i++){
            if(i==1)
            dp[i]=dp[0];
            else
            dp[i]=dp[i-1]+dp[i-2];
        }
        return dp[n];
    }
};
```

## 2 Min Cost Climbing Stairs

You are given an integer array cost where cost[i] is the cost of ith step on a staircase. Once you pay the cost, you can either climb one or two steps.

You can either start from the step with index 0, or the step with index 1.

Return the minimum cost to reach the top of the floor.

```cpp
class Solution {
public:
    int minCostClimbingStairs(vector<int>& cost) {
        vector<int>dp(cost.size()+1,INT_MAX);
        dp[0]=dp[1]=0;
        for(int i=2;i<cost.size()+1;i++){
            dp[i]=min(dp[i-1]+cost[i-1],dp[i-2]+cost[i-2]);
        }
        return dp[cost.size()];
    }
};
```

## 3 N-th Tribonacci Number

The Tribonacci sequence Tn is defined as follows: 

T0 = 0, T1 = 1, T2 = 1, and Tn+3 = Tn + Tn+1 + Tn+2 for n >= 0.

Given n, return the value of Tn.

```cpp
class Solution {
public:
    int tribonacci(int n) {
        vector<int>dp(n+1);
        dp[0]=0;
        if(n>=1)
        dp[1]=1;
        if(n>=2)
        dp[2]=1;
        for(int i=3;i<=n;i++)
        dp[i]=dp[i-1]+dp[i-2]+dp[i-3];
        return dp[n];
    }
};
```

## 4 House Robber

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security systems connected and it will automatically contact the police if two adjacent houses were broken into on the same night.

Given an integer array nums representing the amount of money of each house, return the maximum amount of money you can rob tonight without alerting the police.

```cpp
class Solution {
public:
    int rob(vector<int>& nums) {
        vector<int>dp(nums.size(),0);
        dp[0]=nums[0];
        if(nums.size()>=2)
        dp[1]=max(dp[0],nums[1]);
        for(int i=2;i<nums.size();i++)
        dp[i]=max(dp[i-1],dp[i-2]+nums[i]);
        return dp[nums.size()-1];
    }
};
```

## 5 House Robber II

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed. All houses at this place are arranged in a circle. That means the first house is the neighbor of the last one. Meanwhile, adjacent houses have a security system connected, and it will automatically contact the police if two adjacent houses were broken into on the same night.

Given an integer array nums representing the amount of money of each house, return the maximum amount of money you can rob tonight without alerting the police.

```cpp
class Solution {
public:
    int rob(vector<int>& nums) {
        if(nums.size()==1)
        return nums[0];

        vector<int>dp1(nums.size(),0);
        dp1[0]=0;
        if(nums.size()>=2)
        dp1[1]=nums[1];
        for(int i=2;i<nums.size();i++)
        dp1[i]=max(dp1[i-1],dp1[i-2]+nums[i]);
        int max1=dp1[nums.size()-1];
        
        nums.pop_back();
        vector<int>dp2(nums.size(),0);
        dp2[0]=nums[0];
        if(nums.size()>=2)
        dp2[1]=max(dp2[0],nums[1]);
        for(int i=2;i<nums.size();i++)
        dp2[i]=max(dp2[i-1],dp2[i-2]+nums[i]);
        int max2=dp2[nums.size()-1];
        return max(max1,max2);
    }
};
```

## 6 Longest Palindromic Substring

Given a string s, return the longest palindromic substring in s.

```cpp
class Solution {
public:
    string longestPalindrome(string s) {
        vector<vector<bool>>dp(s.size(),vector<bool>(s.size(),0));
        int maxlen=1,idx=0;
        for(int i=0;i<s.size();i++)
        dp[i][i]=1;
        for(int i=0;i<s.size()-1;i++){
            if(s[i]==s[i+1])
            {
                dp[i][i+1]=1;
                maxlen=2;
                idx=i;
            }
        }
        for(int len=3;len<=s.size();len++){
            for(int i=0;i<s.size()-len+1;i++){
                int j=i+len-1;
                if(s[i]==s[j] && dp[i+1][j-1])
                {
                    dp[i][j]=1;
                    if(maxlen<len){
                        maxlen=len;
                        idx=i;
                    }
                }
            }
        }
        return s.substr(idx,maxlen);
    }
};
```
