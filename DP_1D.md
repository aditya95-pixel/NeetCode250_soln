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

## 7 Palindromic Substrings

Given a string s, return the number of palindromic substrings in it.

A string is a palindrome when it reads the same backward as forward.

A substring is a contiguous sequence of characters within the string.

```cpp
class Solution {
public:
    int countSubstrings(string s) {
        int cnt=0;
        vector<vector<bool>>dp(s.size(),vector<bool>(s.size(),0));
        for(int i=0;i<s.size();i++)
        {
            dp[i][i]=1;
            cnt++;
        }
        for(int i=0;i<s.size()-1;i++)
        {
            if(s[i]==s[i+1])
            {
                dp[i][i+1]=1;
                cnt++;
            }
        }
        for(int len=3;len<=s.size();len++){
            for(int i=0;i<s.size()-len+1;i++){
                int j=i+len-1;
                if(s[i]==s[j] && dp[i+1][j-1])
                {
                    dp[i][j]=1;
                    cnt++;
                }
            }
        }
        return cnt;
    }
};
```

## 8 Decode Ways

You have intercepted a secret message encoded as a string of numbers. The message is decoded via the following mapping:

"1" -> 'A'

"2" -> 'B'

...

"25" -> 'Y'

"26" -> 'Z'

However, while decoding the message, you realize that there are many different ways you can decode the message because some codes are contained in other codes ("2" and "5" vs "25").

For example, "11106" can be decoded into:

"AAJF" with the grouping (1, 1, 10, 6)
"KJF" with the grouping (11, 10, 6)
The grouping (1, 11, 06) is invalid because "06" is not a valid code (only "6" is valid).
Note: there may be strings that are impossible to decode.

Given a string s containing only digits, return the number of ways to decode it. If the entire string cannot be decoded in any valid way, return 0.

The test cases are generated so that the answer fits in a 32-bit integer.

```cpp
class Solution {
public:
    int numDecodings(string s) {
        vector<int>dp(s.size()+1,0);
        dp[0]=1;
        for(int i=0;i<s.size();i++){
            dp[i+1]=dp[i];
            if(i==0 && s[0]=='0')
                return 0;
            else if(s[i]=='0' && (s[i-1]>'2' || s[i-1]=='0'))
                return 0;
            else if(s[i]=='0')
                dp[i+1]=dp[i-1];
            else if(i-1>=0){
                string sub=s.substr(i-1,2);
                if(sub>="10" && sub<="26")
                dp[i+1]+=dp[i-1];
            }
        }
        return dp[s.size()];
    }
};
```

## 9 Coin Change

You are given an integer array coins representing coins of different denominations and an integer amount representing a total amount of money.

Return the fewest number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return -1.

You may assume that you have an infinite number of each kind of coin.

```cpp
class Solution {
public:
    int coinChange(vector<int>& coins, int amount) {
        vector<int>dp(amount+1,INT_MAX);
        dp[0]=0;
        for(int i=0;i<coins.size();i++){
            for(int j=coins[i];j<=amount;j++)
            {
                if(dp[j-coins[i]]!=INT_MAX)
                    dp[j]=min(dp[j],dp[j-coins[i]]+1);
            }
        }
        return (dp[amount]!=INT_MAX)?dp[amount]:-1;
    }
};
```

## 10 Maximum Product Subarray

Given an integer array nums, find a subarray that has the largest product, and return the product.

The test cases are generated so that the answer will fit in a 32-bit integer.

Note that the product of an array with a single element is the value of that element.

```cpp
class Solution {
public:
    int maxProduct(vector<int>& nums) {
        int maxprod=INT_MIN,leftRight=1,rightLeft=1;
        for(int i=0;i<nums.size();i++){
            if(leftRight==0)
            leftRight=1;
            if(rightLeft==0)
            rightLeft=1;
            leftRight*=nums[i];
            rightLeft*=nums[nums.size()-i-1];
            maxprod=max({maxprod,leftRight,rightLeft});
        }
        return maxprod;
    }
};
```

## 11 Word Break

Given a string s and a dictionary of strings wordDict, return true if s can be segmented into a space-separated sequence of one or more dictionary words.

Note that the same word in the dictionary may be reused multiple times in the segmentation.

```cpp
class Solution {
public:
    bool wordBreak(string s, vector<string>& wordDict) {
        set<string>seto(wordDict.begin(),wordDict.end());
        vector<bool>dp(s.size()+1,0);
        dp[0]=1;
        for(int i=0;i<s.size();i++){
            string sub;
            if(!dp[i])
            continue;
            for(int j=i;j<s.size();j++){
                sub+=s[j];
                if(seto.count(sub) && dp[i])
                dp[j+1]=1;
            }
        }
        return dp.back();
    }
};
```

## 12 Longest Increasing Subsequence

Given an integer array nums, return the length of the longest strictly increasing subsequence.

```cpp
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        vector<int>dp(nums.size(),1);
        int maxlen=1;
        for(int i=0;i<nums.size();i++){
            for(int j=0;j<i;j++)
            {
                if(nums[i]>nums[j])
                dp[i]=max(dp[i],dp[j]+1);
                maxlen=max(maxlen,dp[i]);
            }
        }
        return maxlen;
    }
};
```

## 13 Partition Equal Subset Sum

Given an integer array nums, return true if you can partition the array into two subsets such that the sum of the elements in both subsets is equal or false otherwise.

```cpp
class Solution {
public:
    bool canPartition(vector<int>& nums) {
        int sum=accumulate(nums.begin(),nums.end(),0);
        if(sum%2!=0)
        return 0;
        int target=sum/2;
        vector<vector<bool>>dp(nums.size()+1,vector<bool>(target+1,0));
        dp[0][0]=1;
        for(int i=1;i<=nums.size();i++){
            for(int j=0;j<=target;j++){
                dp[i][j]=dp[i-1][j];
                if(dp[i][j])
                continue;
                if(j-nums[i-1]>=0 && dp[i-1][j-nums[i-1]])
                dp[i][j]=1;
            }
        }
        return dp[nums.size()][target];
    }
};
```

## 14 Combination Sum IV

Given an array of distinct integers nums and a target integer target, return the number of possible combinations that add up to target.

The test cases are generated so that the answer can fit in a 32-bit integer.

```cpp
class Solution {
public:
    int combinationSum4(vector<int>& nums, int target) {
        sort(nums.begin(),nums.end());
        vector<uint>dp(target+1,0);
        dp[0]=1;
        for(int j=1;j<=target;j++){
            for(int i=0;i<nums.size();i++){
                if(j-nums[i]>=0)
                dp[j]+=dp[j-nums[i]];
                else
                break;
            }
        }
        return dp[target];
    }
};
```

## 15 Perfect Squares

Given an integer n, return the least number of perfect square numbers that sum to n.

A perfect square is an integer that is the square of an integer; in other words, it is the product of some integer with itself. For example, 1, 4, 9, and 16 are perfect squares while 3 and 11 are not.

```cpp
class Solution {
public:
    int numSquares(int n) {
        vector<int>sqs;
        for(int i=1;i<=sqrt(n);i++)
        sqs.push_back(i*i);
        vector<int>dp(n+1,INT_MAX);
        dp[0]=0;
        for(int num=1;num<=n;num++){
            for(int i=0;i<sqs.size();i++){
                if(num-sqs[i]>=0 && dp[num-sqs[i]]!=INT_MAX)
                dp[num]=min(dp[num],1+dp[num-sqs[i]]);
                else if(num-sqs[i]<0)
                break;
            }
        }
        return dp[n];
    }
};
```

## 16 Integer Break

Given an integer n, break it into the sum of k positive integers, where k >= 2, and maximize the product of those integers.

Return the maximum product you can get.

```cpp
class Solution {
public:
    int integerBreak(int n) {
        vector<int>dp(n+1,0);
        for(int i=2;i<=n;i++){
            for(int j=1;j<i;j++)
                dp[i]=max({dp[i],j*dp[i-j],j*(i-j)});
        }
        return dp[n];
    }
};
```
