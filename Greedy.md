## 1 Lemonade Change

At a lemonade stand, each lemonade costs $5. Customers are standing in a queue to buy from you and order one at a time (in the order specified by bills). Each customer will only buy one lemonade and pay with either a $5, $10, or $20 bill. You must provide the correct change to each customer so that the net transaction is that the customer pays $5.

Note that you do not have any change in hand at first.

Given an integer array bills where bills[i] is the bill the ith customer pays, return true if you can provide every customer with the correct change, or false otherwise.

```cpp
class Solution {
public:
    bool lemonadeChange(vector<int>& bills) {
        int curr5=0,curr10=0;
        for(int i=0;i<bills.size();i++){
            if(bills[i]==5)
            curr5++;
            else if(bills[i]==10)
            {
                if(curr5==0)
                return 0;
                curr5--;
                curr10++;
            }
            else if(bills[i]==20){
                if(curr10>0 && curr5>0)
                {
                    curr5--;
                    curr10--;
                }else if(curr5>2)
                    curr5-=3;
                else
                return 0;
            }
        }
        return 1;
    }
};
```

## 2 Maximum Subarray

Given an integer array nums, find the subarray with the largest sum, and return its sum.

```cpp
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int maxsum=nums[0],sum=nums[0];
        for(int i=1;i<nums.size();i++){
            sum=max(nums[i],sum+nums[i]);
            maxsum=max(maxsum,sum);
        }
        return maxsum;
    }
};
```

## 3 Maximum Sum Circular Subarray

Given a circular integer array nums of length n, return the maximum possible sum of a non-empty subarray of nums.

A circular array means the end of the array connects to the beginning of the array. Formally, the next element of nums[i] is nums[(i + 1) % n] and the previous element of nums[i] is nums[(i - 1 + n) % n].

A subarray may only include each element of the fixed buffer nums at most once. Formally, for a subarray nums[i], nums[i + 1], ..., nums[j], there does not exist i <= k1, k2 <= j with k1 % n == k2 % n.

```cpp
class Solution {
public:
    int maxSubarraySumCircular(vector<int>& nums) {
        int maxsum=nums[0],minsum=nums[0],sum1=nums[0],sum2=nums[0],totalsum=nums[0];
        for(int i=1;i<nums.size();i++){
            int num=nums[i];
            sum1=max(num,sum1+num);
            maxsum=max(maxsum,sum1);
            sum2=min(num,sum2+num);
            minsum=min(minsum,sum2);
            totalsum+=num;
        }
        if(totalsum==minsum)
            return maxsum;
        else
            return max(maxsum,totalsum-minsum);
    }
};
```

## 4 Longest Turbulent Subarray

Given an integer array arr, return the length of a maximum size turbulent subarray of arr.

A subarray is turbulent if the comparison sign flips between each adjacent pair of elements in the subarray.

More formally, a subarray [arr[i], arr[i + 1], ..., arr[j]] of arr is said to be turbulent if and only if:

For i <= k < j:
arr[k] > arr[k + 1] when k is odd, and
arr[k] < arr[k + 1] when k is even.

Or, for i <= k < j:
arr[k] > arr[k + 1] when k is even, and
arr[k] < arr[k + 1] when k is odd.

```cpp
class Solution {
public:
    int maxTurbulenceSize(vector<int>& arr) {
        if(arr.size()==1)
        return 1;
        int maxlen=0,len;
        int chk;
        if(arr[0]<arr[1])
        chk=1;
        else if(arr[0]>arr[1])
        chk=-1;
        else
        chk=0;
        if(chk==1 || chk==-1)
        len=2;
        else
        len=1;
        maxlen=len;
        for(int i=2;i<arr.size();i++){
            if(arr[i]>arr[i-1] && (chk==-1 || chk==0))
            {
                len++;
                chk=1;
            }
            else if(arr[i]<arr[i-1] && (chk==1 || chk==0)){
                len++;
                chk=-1;
            }
            else if(arr[i]>arr[i-1])
            {
                chk=1;
                len=2;
            }
            else if(arr[i]<arr[i-1]){
                chk=-1;
                len=2;
            }
            else if(arr[i]==arr[i-1])
            {
                chk=0;
                len=1;
            }
            maxlen=max(maxlen,len);
        }
        return maxlen;
    }
};
```

## 5 Jump Game

You are given an integer array nums. You are initially positioned at the array's first index, and each element in the array represents your maximum jump length at that position.

Return true if you can reach the last index, or false otherwise.

```cpp
class Solution {
public:
    bool canJump(vector<int>& nums) {
        int reachable=0;
        for(int i=0;i<nums.size()-1;i++){
            if(i>reachable)
            return 0;
            reachable=max(reachable,i+nums[i]);
        }
        return reachable>=nums.size()-1;
    }
};
```

## 6 Jump Game II

You are given a 0-indexed array of integers nums of length n. You are initially positioned at index 0.

Each element nums[i] represents the maximum length of a forward jump from index i. In other words, if you are at index i, you can jump to any index (i + j) where:

0 <= j <= nums[i] and
i + j < n
Return the minimum number of jumps to reach index n - 1. The test cases are generated such that you can reach index n - 1.

```cpp
class Solution {
public:
    int jump(vector<int>& nums) {
        if(nums.size()==1)
        return 0;
        int end=0,cnt=0,reachable=0;
        for(int i=0;i<nums.size();i++){
            reachable=max(reachable,i+nums[i]);
            if(i==end){
                cnt++;
                end=reachable;
                if(end>=nums.size()-1)
                return cnt;
            }
        }
        return cnt;
    }
};
```

## 7 Jump Game VII

You are given a 0-indexed binary string s and two integers minJump and maxJump. In the beginning, you are standing at index 0, which is equal to '0'. You can move from index i to index j if the following conditions are fulfilled:

i + minJump <= j <= min(i + maxJump, s.length - 1), and
s[j] == '0'.
Return true if you can reach index s.length - 1 in s, or false otherwise.

```cpp
class Solution {
public:
    bool canReach(string s, int minJump, int maxJump) {
        vector<int>dp(s.size(),0);
        dp[0]=1;
        int curr=0;
        for(int i=1;i<s.size();i++)
        {
            if(i-minJump>=0 && dp[i-minJump])
            curr++;
            if(i-maxJump>0 && dp[i-maxJump-1])
            curr--;
            if(s[i]=='0' && curr>0)
            dp[i]=curr;
        }
        return dp.back();
    }
};
```
