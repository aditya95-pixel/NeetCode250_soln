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
