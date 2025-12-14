### 1 Binary Search

Given an array of integers nums which is sorted in ascending order, and an integer target, write a function to search target in nums. If target exists, then return its index. Otherwise, return -1.

You must write an algorithm with O(log n) runtime complexity.

```cpp
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int l=0,h=nums.size()-1;
        while(l<=h){
            int mid=l+(h-l)/2;
            if(nums[mid]==target)
            return mid;
            else if(nums[mid]>target)
            h=mid-1;
            else
            l=mid+1;
        }
        return -1;
    }
};
```

### 2 Search Insert Position

Given a sorted array of distinct integers and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order.

You must write an algorithm with O(log n) runtime complexity.

```cpp
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {
        int l=0,h=nums.size()-1;
        while(l<=h){
            int mid=l+(h-l)/2;
            if(nums[mid]>=target)
            h=mid-1;
            else
            l=mid+1;
        }
        return l;
    }
};
```

### 3 Guess Number Higher or Lower

We are playing the Guess Game. The game is as follows:

I pick a number from 1 to n. You have to guess which number I picked (the number I picked stays the same throughout the game).

Every time you guess wrong, I will tell you whether the number I picked is higher or lower than your guess.

You call a pre-defined API int guess(int num), which returns three possible results:

-1: Your guess is higher than the number I picked (i.e. num > pick).
1: Your guess is lower than the number I picked (i.e. num < pick).
0: your guess is equal to the number I picked (i.e. num == pick).
Return the number that I picked.

```cpp
/** 
 * Forward declaration of guess API.
 * @param  num   your guess
 * @return 	     -1 if num is higher than the picked number
 *			      1 if num is lower than the picked number
 *               otherwise return 0
 * int guess(int num);
 */

class Solution {
public:
    int guessNumber(int n) {
        int l=1,h=n;
        while(l<=h){
            int mid=l+(h-l)/2;
            if(guess(mid)==1)
            l=mid+1;
            else if(guess(mid)==-1)
            h=mid-1;
            else
            return mid;
        }
        return -1;
    }
};
```

### 4 Sqrt(x)

Given a non-negative integer x, return the square root of x rounded down to the nearest integer. The returned integer should be non-negative as well.

You must not use any built-in exponent function or operator.

For example, do not use pow(x, 0.5) in c++ or x ** 0.5 in python.

```cpp
class Solution {
public:
    int mySqrt(int x) {
        int l=0,h=x;
        while(l<=h){
            int mid=l+(h-l)/2;
            if((long long)mid*mid==x)
            return mid;
            else if((long long)mid*mid<x)
            l=mid+1;
            else
            h=mid-1;
        }
        return h;
    }
};
```

### 5 Search a 2D Matrix

You are given an m x n integer matrix matrix with the following two properties:

Each row is sorted in non-decreasing order.
The first integer of each row is greater than the last integer of the previous row.
Given an integer target, return true if target is in matrix or false otherwise.

You must write a solution in O(log(m * n)) time complexity.

```cpp
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        for(int i=0;i<matrix.size();i++){
            int l=0,h=matrix[i].size()-1;
            while(l<=h){
                int mid=l+(h-l)/2;
                if(matrix[i][mid]==target)
                return true;
                else if(matrix[i][mid]>target)
                h=mid-1;
                else
                l=mid+1;
            }
        }
        return false;
    }
};
```

### 6 Koko Eating Bananas

Koko loves to eat bananas. There are n piles of bananas, the ith pile has piles[i] bananas. The guards have gone and will come back in h hours.

Koko can decide her bananas-per-hour eating speed of k. Each hour, she chooses some pile of bananas and eats k bananas from that pile. If the pile has less than k bananas, she eats all of them instead and will not eat any more bananas during this hour.

Koko likes to eat slowly but still wants to finish eating all the bananas before the guards return.

Return the minimum integer k such that she can eat all the bananas within h hours.

```cpp
class Solution {
public:
    bool solve(vector<int>&piles,int cnt,int h){
        long long hrs=0;
        for(int i=0;i<piles.size();i++){
            if(piles[i]%cnt==0)
            hrs+=piles[i]/cnt;
            else
            hrs+=piles[i]/cnt+1;
        }
        return hrs<=h;
    }
    int minEatingSpeed(vector<int>& piles, int h) {
        sort(piles.begin(),piles.end());
        int l=1,r=piles[piles.size()-1];
        int res=r;
        while(l<=r){
            int mid=l+(r-l)/2;
            if(solve(piles,mid,h))
            {
                res=mid;
                r=mid-1;
            }
            else
            l=mid+1;
        }
        return res;
    }
};
```

### 7 Capacity To Ship Packages Within D Days

A conveyor belt has packages that must be shipped from one port to another within days days.

The ith package on the conveyor belt has a weight of weights[i]. Each day, we load the ship with packages on the conveyor belt (in the order given by weights). We may not load more weight than the maximum weight capacity of the ship.

Return the least weight capacity of the ship that will result in all the packages on the conveyor belt being shipped within days days.

```cpp
class Solution {
public:
    bool solve(vector<int>&weights,int cap,int days){
        int cnt=1,temp=0;
        for(int i=0;i<weights.size();i++){
            temp+=weights[i];
            if(temp>cap){
                temp=weights[i];
                cnt++;
            }
        }
        return cnt<=days;
    }
    int shipWithinDays(vector<int>& weights, int days) {
        int l=*max_element(weights.begin(),weights.end()),
        h=accumulate(weights.begin(),weights.end(),0);
        int res=h;
        while(l<=h){
            int mid=l+(h-l)/2;
            if(solve(weights,mid,days))
            {
                res=mid;
                h=mid-1;
            }
            else
            l=mid+1;
        }
        return res;
    }
};
```

### 8 Find Minimum in Rotated Sorted Array

Suppose an array of length n sorted in ascending order is rotated between 1 and n times. For example, the array nums = [0,1,2,4,5,6,7] might become:

[4,5,6,7,0,1,2] if it was rotated 4 times.
[0,1,2,4,5,6,7] if it was rotated 7 times.
Notice that rotating an array [a[0], a[1], a[2], ..., a[n-1]] 1 time results in the array [a[n-1], a[0], a[1], a[2], ..., a[n-2]].

Given the sorted rotated array nums of unique elements, return the minimum element of this array.

You must write an algorithm that runs in O(log n) time.

```cpp
class Solution {
public:
    int findMin(vector<int> &nums) {
        int l=0,h=nums.size()-1;
        int res=INT_MAX;
        while(l<=h){
            int mid=l+(h-l)/2;
            res=min(res,nums[mid]);
            if(nums[mid]>nums[h])
                l=mid+1;
            else
                h=mid-1;
        }
        return res;
    }
};
```

### 9 Search in Rotated Sorted Array

There is an integer array nums sorted in ascending order (with distinct values).

Prior to being passed to your function, nums is possibly left rotated at an unknown index k (1 <= k < nums.length) such that the resulting array is [nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]] (0-indexed). For example, [0,1,2,4,5,6,7] might be left rotated by 3 indices and become [4,5,6,7,0,1,2].

Given the array nums after the possible rotation and an integer target, return the index of target if it is in nums, or -1 if it is not in nums.

You must write an algorithm with O(log n) runtime complexity.

```cpp
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int l=0,h=nums.size()-1;
        while(l<=h){
            int mid=l+(h-l)/2;
            if(nums[mid]==target)
            return mid;
            if(nums[l]<nums[mid]){
                if(nums[l]<=target && target<nums[mid])
                h=mid-1;
                else
                l=mid+1;
            }
            else if(nums[mid]<nums[l]){
                if(target<=nums[h] && target>nums[mid])
                l=mid+1;
                else
                h=mid-1;
            }
            else
            l++;
        }
        return -1;
    }
};
```

### 10 Search in Rotated Sorted Array II

There is an integer array nums sorted in non-decreasing order (not necessarily with distinct values).

Before being passed to your function, nums is rotated at an unknown pivot index k (0 <= k < nums.length) such that the resulting array is [nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]] (0-indexed). For example, [0,1,2,4,4,4,5,6,6,7] might be rotated at pivot index 5 and become [4,5,6,6,7,0,1,2,4,4].

Given the array nums after the rotation and an integer target, return true if target is in nums, or false if it is not in nums.

You must decrease the overall operation steps as much as possible.

```cpp
class Solution {
public:
    bool search(vector<int>& nums, int target) {
        int l=0,h=nums.size()-1;
        while(l<=h){
            int mid=l+(h-l)/2;
            if(nums[mid]==target)
            return 1;
            if(nums[l]<nums[mid]){
                if(nums[l]<=target && target<nums[mid])
                h=mid-1;
                else
                l=mid+1;
            }
            else if(nums[mid]<nums[l]){
                if(target<=nums[h] && target>nums[mid])
                l=mid+1;
                else
                h=mid-1;
            }
            else
            l++;
        }
        return 0;
    }
};
```

### 11 Time Based Key-Value Store

Design a time-based key-value data structure that can store multiple values for the same key at different time stamps and retrieve the key's value at a certain timestamp.

Implement the TimeMap class:

TimeMap() Initializes the object of the data structure.
void set(String key, String value, int timestamp) Stores the key key with the value value at the given time timestamp.
String get(String key, int timestamp) Returns a value such that set was called previously, with timestamp_prev <= timestamp. If there are multiple such values, it returns the value associated with the largest timestamp_prev. If there are no values, it returns "".

```cpp
class TimeMap {
    unordered_map<string,vector<pair<int,string>>>mp;
public:
    TimeMap() {
        
    }
    
    void set(string key, string value, int timestamp) {
        mp[key].push_back({timestamp,value});
    }
    
    string get(string key, int timestamp) {
        int l=0,h=mp[key].size()-1;
        string res="";
        while(l<=h){
            int mid=l+(h-l)/2;
            if(mp[key][mid].first==timestamp)
            return mp[key][mid].second;
            else if(mp[key][mid].first<timestamp){
                res=mp[key][mid].second;
                l=mid+1;
            }
            else
            h=mid-1;
        }
        return res;
    }
};
```

### 12 Split Array Largest Sum

Given an integer array nums and an integer k, split nums into k non-empty subarrays such that the largest sum of any subarray is minimized.

Return the minimized largest sum of the split.

A subarray is a contiguous part of the array.

```cpp
class Solution {
public:
    bool solve(vector<int>& nums, int k,int sum){
        int temp=0,cnt=1;
        for(int i=0;i<nums.size();i++){
            temp+=nums[i];
            if(temp>sum){
                temp=nums[i];
                cnt++;
            }
        }
        return cnt<=k;
    }
    int splitArray(vector<int>& nums, int k) {
        int l=*max_element(nums.begin(),nums.end()),
        h=accumulate(nums.begin(),nums.end(),0);
        int res=h;
        while(l<=h){
            int mid=l+(h-l)/2;
            if(solve(nums,k,mid)){
                res=mid;
                h=mid-1;
            }else
            l=mid+1;
        }
        return res;
    }
};
```

### 13 Median of Two Sorted Arrays

Given two sorted arrays nums1 and nums2 of size m and n respectively, return the median of the two sorted arrays.

The overall run time complexity should be O(log (m+n)).

```cpp
class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        if(nums1.empty()){
            int len=nums2.size();
            if(nums2.size()%2==0)
            return (nums2[len/2-1]+nums2[len/2])/2.0;
            else
            return nums2[len/2];
        }
        else if(nums2.empty()){
            int len=nums1.size();
            if(nums1.size()%2==0)
            return (nums1[len/2-1]+nums1[len/2])/2.0;
            else
            return nums1[len/2];
        }
        if(nums1.size()>nums2.size())
        return findMedianSortedArrays(nums2,nums1);
        int l=0,h=nums1.size();
        while(l<=h){
            int mid1=l+(h-l)/2;
            int mid2=(nums1.size()+nums2.size()+1)/2-mid1;
            int l1=(mid1==0?INT_MIN:nums1[mid1-1]);
            int r1=(mid1==nums1.size()?INT_MAX:nums1[mid1]);
            int l2=(mid2==0?INT_MIN:nums2[mid2-1]);
            int r2=(mid2==nums2.size()?INT_MAX:nums2[mid2]);
            if(l1<=r2 && l2<=r1){
                if((nums1.size()+nums2.size())%2==0)
                return (max(l1,l2)+min(r1,r2))/2.0;
                else
                return max(l1,l2);
            }
            if(l1>r2)
            h=mid1-1;
            else
            l=mid1+1;
        }
        return 0;
    }
};
```

### 14 Find in Mountain Array

(This problem is an interactive problem.)

You may recall that an array arr is a mountain array if and only if:

arr.length >= 3
There exists some i with 0 < i < arr.length - 1 such that:
arr[0] < arr[1] < ... < arr[i - 1] < arr[i]
arr[i] > arr[i + 1] > ... > arr[arr.length - 1]
Given a mountain array mountainArr, return the minimum index such that mountainArr.get(index) == target. If such an index does not exist, return -1.

You cannot access the mountain array directly. You may only access the array using a MountainArray interface:

MountainArray.get(k) returns the element of the array at index k (0-indexed).
MountainArray.length() returns the length of the array.
Submissions making more than 100 calls to MountainArray.get will be judged Wrong Answer. Also, any solutions that attempt to circumvent the judge will result in disqualification.

```cpp
/**
 * // This is the MountainArray's API interface.
 * // You should not implement it, or speculate about its implementation
 * class MountainArray {
 *   public:
 *     int get(int index);
 *     int length();
 * };
 */

class Solution {
    unordered_map<int,int>mp;
public:
    int find(int idx,MountainArray &mountainArr){
        if(mp.count(idx))
        return mp[idx];
        else
        return mp[idx]=mountainArr.get(idx);
    }
    int findInMountainArray(int target, MountainArray &mountainArr) {
        int l=0,h=mountainArr.length()-1;
        int maxidx=-1;
        while(l<=h){
            int mid=l+(h-l)/2;
            if(mid-1>=0 && find(mid,mountainArr)>find(mid-1,mountainArr) && 
            mid+1<=mountainArr.length()-1 && find(mid,mountainArr)>find(mid+1,mountainArr))
            {
                maxidx=mid;
                break;
            }
            else if(mid-1>=0 && find(mid,mountainArr)<find(mid-1,mountainArr))
            h=mid-1;
            else if(mid+1<=mountainArr.length()-1 && find(mid,mountainArr)<find(mid+1,mountainArr))
            l=mid+1;
        }
        if(find(maxidx,mountainArr)==target)
        return maxidx;
        l=0;
        h=maxidx-1;
        while(l<=h){
            int mid=l+(h-l)/2;
            if(find(mid,mountainArr)==target)
            return mid;
            else if(find(mid,mountainArr)>target)
            h=mid-1;
            else
            l=mid+1;
        }
        l=maxidx+1;
        h=mountainArr.length()-1;
        while(l<=h){
            int mid=l+(h-l)/2;
            if(find(mid,mountainArr)==target)
            return mid;
            else if(find(mid,mountainArr)>target)
            l=mid+1;
            else
            h=mid-1;
        }
        return -1;
    }
};
```
