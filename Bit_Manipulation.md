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
