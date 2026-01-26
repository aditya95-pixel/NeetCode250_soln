## 1 Single Number

Given a non-empty array of integers nums, every element appears twice except for one. Find that single one.

You must implement a solution with a linear runtime complexity and use only constant extra space.

```cpp
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        int xoro=nums[0];
        for(int i=1;i<nums.size();i++)
        xoro^=nums[i];
        return xoro;
    }
};
```

## 2 Number of 1 Bits

Given a positive integer n, write a function that returns the number of set bits in its binary representation (also known as the Hamming weight).

```cpp
class Solution {
public:
    int hammingWeight(int x) {
        int cnt=0;
        while(x){
            x&=(x-1);
            cnt++;
        }
        return cnt;
    }
};
```

## 3 Counting Bits

Given an integer n, return an array ans of length n + 1 such that for each i (0 <= i <= n), ans[i] is the number of 1's in the binary representation of i.

```cpp
class Solution {
public:
    vector<int> countBits(int n) {
        vector<int>dp(n+1,0);
        for(int i=0;i<=n;i++){
            int x=i;
            int cnt=0;
            while(x){
                x&=(x-1);
                cnt++;
                if(dp[x]){
                    cnt+=dp[x];
                    break;
                }
            }
            dp[i]=cnt;
        }
        return dp;
    }
};
```

## 4 Add Binary

Given two binary strings a and b, return their sum as a binary string.

```cpp
class Solution {
public:
    string addBinary(string a, string b) {
        reverse(a.begin(),a.end());
        reverse(b.begin(),b.end());
        int carry=0;
        int i=0,j=0;
        string res;
        while(i<a.size() && j<b.size()){
            int dig1=a[i]-'0',dig2=b[j]-'0';
            if(carry==0){
                if(dig1==1 && dig2==1){
                    res+='0';
                    carry=1;
                }else if(dig1==1)
                res+='1';
                else if(dig2==1)
                res+='1';
                else
                res+='0';
            }else{
                if(dig1==1 && dig2==1){
                    res+='1';
                    carry=1;
                }else if(dig1==1)
                res+='0';
                else if(dig2==1)
                res+='0';
                else
                {
                    res+='1';
                    carry=0;
                }
            }
            i++;
            j++;
        }
        while(i<a.size()){
            int dig1=a[i]-'0';
            if(carry==0){
                if(dig1==1){
                    res+='1';
                }
                else
                res+='0';
            }else{
                if(dig1==1)
                res+='0';
                else
                {
                    res+='1';
                    carry=0;
                }
            }
            i++;
        }
        while(j<b.size()){
            int dig1=b[j]-'0';
            if(carry==0){
                if(dig1==1){
                    res+='1';
                }
                else
                res+='0';
            }else{
                if(dig1==1)
                res+='0';
                else
                {
                    res+='1';
                    carry=0;
                }
            }
            j++;
        }
        if(carry)
        res+='1';
        reverse(res.begin(),res.end());
        return res;
    }
};
```

## 5 Reverse Bits

Reverse bits of a given 32 bits signed integer.

```cpp
class Solution {
public:
    uint32_t reverseBits(uint32_t n) {
        uint32_t res=0;
        for(int bit=0;bit<32;bit++){
            int revbit=32-bit-1;
            if((n>>bit)&1)
                res|=(1<<revbit);
        }
        return res;
    }
};
```

## 6 Missing Number

Given an array nums containing n distinct numbers in the range [0, n], return the only number in the range that is missing from the array.

```cpp
class Solution {
public:
    int missingNumber(vector<int>& nums) {
        int xoro=0;
        for(int i=1;i<=nums.size();i++)
        xoro^=i;
        for(int i=0;i<nums.size();i++)
        xoro^=nums[i];
        return xoro;
    }
};
```

## 7 Sum of Two Integers

Given two integers a and b, return the sum of the two integers without using the operators + and -.

```cpp
class Solution {
public:
    int getSum(int a, int b) {
        int res=0;
        bool carry=0;
        for(int bit=0;bit<32;bit++){
            bool ona=(a>>bit)&1,onb=(b>>bit)&1;
            if(carry){
                if(ona && onb)
                    res|=(1<<bit);
                else if(!ona && !onb)
                {
                    res|=(1<<bit);
                    carry=0;
                }
            }else{
                if(ona && onb)
                    carry=1;
                else if(ona || onb)
                    res|=(1<<bit);
            }
        }
        return res;
    }
};
```

## 8 Reverse Integer

Given a signed 32-bit integer x, return x with its digits reversed. If reversing x causes the value to go outside the signed 32-bit integer range [-231, 231 - 1], then return 0.

Assume the environment does not allow you to store 64-bit integers (signed or unsigned).

```cpp
class Solution {
public:
    int reverse(int x) {
        const int MAX=2147483647,MIN=-2147483648;
        int res=0;
        while(x!=0){
            int dig=x%10;
            if(res>MAX/10 || (res==MAX/10 && dig>MAX%10))
                return 0;
            if(res<MIN/10 || (res==MIN/10 && dig<MIN%10))
                return 0;
            res=res*10+dig;
            x/=10;
        }
        return res;
    }
};
```

## 9 Bitwise AND of Numbers Range

Given two integers left and right that represent the range [left, right], return the bitwise AND of all numbers in this range, inclusive.

```cpp
class Solution {
public:
    int rangeBitwiseAnd(int left, int right) {
        int shifts=0;
        while(left<right){
            left>>=1;
            right>>=1;
            shifts++;
        }
        return left<<shifts;
    }
};
```

## 10 Minimum Array End

You are given two integers n and x. You have to construct an array of positive integers nums of size n where for every 0 <= i < n - 1, nums[i + 1] is greater than nums[i], and the result of the bitwise AND operation between all elements of nums is x.

Return the minimum possible value of nums[n - 1].

```cpp
class Solution {
public:
    long long minEnd(int n, int x) {
        long long res=x;
        while(n-->1)
            res=(res+1)|x;
        return res;
    }
};
```

## 11 Minimum Number of Work Sessions to Finish the Tasks

There are n tasks assigned to you. The task times are represented as an integer array tasks of length n, where the ith task takes tasks[i] hours to finish. A work session is when you work for at most sessionTime consecutive hours and then take a break.

You should finish the given tasks in a way that satisfies the following conditions:

If you start a task in a work session, you must complete it in the same work session.
You can start a new task immediately after finishing the previous one.
You may complete the tasks in any order.
Given tasks and sessionTime, return the minimum number of work sessions needed to finish all the tasks following the conditions above.

The tests are generated such that sessionTime is greater than or equal to the maximum element in tasks[i].

 ```cpp
class Solution {
public:
    int minSessions(vector<int>& tasks, int sessionTime) {
        int n=tasks.size();
        vector<pair<int,int>>dp(1<<n,{INT_MAX,0});
        for(int i=0;i<n;i++){
            if(tasks[i]==sessionTime){
                dp[1<<i].first=1;
                dp[1<<i].second=0;
            }else{
                dp[1<<i].first=0;
                dp[1<<i].second=tasks[i];
            }
        }
        for(int mask=1;mask<1<<n;mask++){
            if(((mask-1)&mask)==0)
            continue;
            for(int i=0;i<n;i++){
                if((mask>>i)&1){
                    int other=mask^(1<<i);
                    pair<int,int>cand;
                    if(dp[other].second+tasks[i]<sessionTime){
                        cand.first=dp[other].first;
                        cand.second=dp[other].second+tasks[i];
                    }
                    else if(dp[other].second+tasks[i]==sessionTime){
                        cand.first=dp[other].first+1;
                        cand.second=0;
                    }else{
                        cand.first=dp[other].first+1;
                        cand.second=tasks[i];
                    }
                    if(cand.first<dp[mask].first || (cand.first==dp[mask].first && cand.second<dp[mask].second))
                        dp[mask]=cand;
                }
            }
        }
        return (dp.back().second==0)?dp.back().first:dp.back().first+1;
    }
};
```

## 12 Smallest Sufficient Team

In a project, you have a list of required skills req_skills, and a list of people. The ith person people[i] contains a list of skills that the person has.

Consider a sufficient team: a set of people such that for every required skill in req_skills, there is at least one person in the team who has that skill. We can represent these teams by the index of each person.

For example, team = [0, 1, 3] represents the people with skills people[0], people[1], and people[3].
Return any sufficient team of the smallest possible size, represented by the index of each person. You may return the answer in any order.

It is guaranteed an answer exists.

```cpp
class Solution {
public:
    vector<int> smallestSufficientTeam(vector<string>& req_skills, vector<vector<string>>& people) {
        int n=req_skills.size();
        unordered_map<string,int>skill_index;
        for(int i=0;i<n;i++)
        skill_index[req_skills[i]]=i;
        unordered_map<int,vector<int>>dp;
        for(int i=0;i<people.size();i++){
            int pplskillmask=0;
            for(auto &skill:people[i])
            pplskillmask|=(1<<(skill_index[skill]));
            unordered_map<int,vector<int>>old_dp=dp;
            for(auto &item:old_dp){
                int prev_mask=item.first;
                int combined_mask=(prev_mask|pplskillmask);
                if(!dp.count(combined_mask) || dp[combined_mask].size()>dp[prev_mask].size()+1){
                    vector<int>team=item.second;
                    team.push_back(i);
                    dp[combined_mask]=team;
                }
            }
            dp[pplskillmask]={i};
        }
        return dp[(1<<n)-1];
    }
};
```

## 13 Maximum Length of a Concatenated String with Unique Characters

You are given an array of strings arr. A string s is formed by the concatenation of a subsequence of arr that has unique characters.

Return the maximum possible length of s.

A subsequence is an array that can be derived from another array by deleting some or no elements without changing the order of the remaining elements.

```cpp
class Solution {
public:
    int maxLength(vector<string>& arr) {
        int n=arr.size(),maxlen=0;
        vector<int>dp(1<<n,0);
        for(int i=0;i<arr.size();i++){
            set<char>seto(arr[i].begin(),arr[i].end());
            if(seto.size()<arr[i].size())
            continue;
            for(auto &c:arr[i])
            dp[1<<i]|=(1<<(c-'a'));
            maxlen=max(maxlen,(int)arr[i].size());
        }
        for(int mask=1;mask<1<<n;mask++){
            if(mask&(mask-1)==0)
            continue;
            int curr=0;
            bool chk=1;
            for(int i=0;i<n;i++){
                if((mask>>i)&1)
                {
                    if(curr&dp[1<<i]){
                        chk=0;
                        break;
                    }
                    curr|=dp[1<<i];
                }
            }
            if(!chk)
            continue;
            int len=0;
            for(int i=0;i<26;i++){
                if((curr>>i)&1)
                len++;
            }
            maxlen=max(maxlen,len);
        }
        return maxlen;
    }
};
```

## 14 Maximize Score After N Operations

You are given nums, an array of positive integers of size 2 * n. You must perform n operations on this array.

In the ith operation (1-indexed), you will:

Choose two elements, x and y.
Receive a score of i * gcd(x, y).
Remove x and y from nums.
Return the maximum score you can receive after performing n operations.

The function gcd(x, y) is the greatest common divisor of x and y.

```cpp
class Solution {
public:
    int maxScore(vector<int>& nums) {
        int n=nums.size();
        vector<int>dp(1<<n,0);
        for(int mask=1;mask<1<<n;mask++){
            if((mask&(mask-1))==0)
            continue;
            int cnt=0,num=mask;
            while(num){
                num&=(num-1);
                cnt++;
            }
            if(cnt%2!=0)
            continue;
            for(int i=0;i<n;i++){
                for(int j=0;j<n;j++){
                    if(i==j)
                    continue;
                    if(((mask>>i)&1) && ((mask>>j)&1)){
                        num=(1<<i)|(1<<j);
                        int other=mask^num;
                        dp[mask]=max(dp[mask],dp[other]+cnt/2*gcd(nums[i],nums[j]));            
                    }
                }
            }
        }
        return dp.back();
    }
};
```
