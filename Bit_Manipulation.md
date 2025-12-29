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
