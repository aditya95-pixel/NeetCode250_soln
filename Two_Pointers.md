### 1 Reverse String

Write a function that reverses a string. The input string is given as an array of characters s.

You must do this by modifying the input array in-place with O(1) extra memory.

```cpp
class Solution {
public:
    void reverseString(vector<char>& s) {
        for(int i=0;i<s.size()/2;i++)
        swap(s[i],s[s.size()-i-1]);
    }
};
```

### 2 Valid Palindrome

A phrase is a palindrome if, after converting all uppercase letters into lowercase letters and removing all non-alphanumeric characters, it reads the same forward and backward. Alphanumeric characters include letters and numbers.

Given a string s, return true if it is a palindrome, or false otherwise.

```cpp
class Solution {
public:
    bool isPalindrome(string s) {
        string temp;
        for(auto &c:s){
            if(!isalnum(c))
            continue;
            else if(isdigit(c))
            temp+=c;
            else if(c<='Z' && c>='A')
            temp+=(char)(c+32);
            else if(c<='z' && c>='a')
            temp+=c;
        }
        s=temp;
        for(int i=0;i<s.size()/2;i++){
            if(s[i]!=s[s.size()-i-1])
            return false;
        }
        return true;
    }
};
```

### 3 Valid Palindrome II

Given a string s, return true if the s can be palindrome after deleting at most one character from it.

```cpp
class Solution {
public:
    bool ispalin(string &s,int l,int r){
        while(l<r){
            if(s[l]!=s[r])
            return 0;
            l++;
            r--;
        }
        return 1;
    }
    bool validPalindrome(string s) {
        int l=0,r=s.size()-1;
        while(l<r){
            if(s[l]!=s[r])
            return ispalin(s,l+1,r) || ispalin(s,l,r-1);
            l++;
            r--;
        }
        return 1;
    }
};
```

### 4 Merge Strings Alternately

You are given two strings word1 and word2. Merge the strings by adding letters in alternating order, starting with word1. If a string is longer than the other, append the additional letters onto the end of the merged string.

Return the merged string.

```cpp
class Solution {
public:
    string mergeAlternately(string word1, string word2) {
        bool chk=1;
        int i=0,j=0;
        string res;
        while(i<word1.size() && j<word2.size()){
            if(chk)
            {
                res+=word1[i++];
                chk=!chk;
            }else{
                res+=word2[j++];
                chk=!chk;
            }
        }
        while(i<word1.size())
        res+=word1[i++];
        while(j<word2.size())
        res+=word2[j++];
        return res;
    }
};
```

### 5 Merge Sorted Array

You are given two integer arrays nums1 and nums2, sorted in non-decreasing order, and two integers m and n, representing the number of elements in nums1 and nums2 respectively.

Merge nums1 and nums2 into a single array sorted in non-decreasing order.

The final sorted array should not be returned by the function, but instead be stored inside the array nums1. To accommodate this, nums1 has a length of m + n, where the first m elements denote the elements that should be merged, and the last n elements are set to 0 and should be ignored. nums2 has a length of n.

```cpp
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        int i=m+n-1,j=0;
        while(j<n)
            swap(nums1[i--],nums2[j++]);
        sort(nums1.begin(),nums1.end());
    }
};
```

### 6 Remove Duplicates from Sorted Array

Given an integer array nums sorted in non-decreasing order, remove the duplicates in-place such that each unique element appears only once. The relative order of the elements should be kept the same.

Consider the number of unique elements in nums to be k​​​​​​​​​​​​​​. After removing duplicates, return the number of unique elements k.

The first k elements of nums should contain the unique numbers in sorted order. The remaining elements beyond index k - 1 can be ignored.

```cpp
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        int k=0;
        set<int>s;
        for(int i=0;i<nums.size();i++){
            if(!s.count(nums[i])){
                nums[k++]=nums[i];
                s.insert(nums[i]);
            }
        }
        return k;
    }
};
```

### 7 Two Sum II - Input Array Is Sorted

Given a 1-indexed array of integers numbers that is already sorted in non-decreasing order, find two numbers such that they add up to a specific target number. Let these two numbers be numbers[index1] and numbers[index2] where 1 <= index1 < index2 <= numbers.length.

Return the indices of the two numbers, index1 and index2, added by one as an integer array [index1, index2] of length 2.

The tests are generated such that there is exactly one solution. You may not use the same element twice.

Your solution must use only constant extra space.

```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& numbers, int target) {
        vector<int>res;
        int l=0,r=numbers.size()-1;
        while(l<r){
            if(numbers[l]+numbers[r]==target)
            {
                res.push_back(l+1);
                res.push_back(r+1);
                return res;
            }else if(numbers[l]+numbers[r]<target)
            l++;
            else
            r--;
        }
        return res;
    }
};
```

### 8 3Sum

Given an integer array nums, return all the triplets [nums[i], nums[j], nums[k]] such that i != j, i != k, and j != k, and nums[i] + nums[j] + nums[k] == 0.

Notice that the solution set must not contain duplicate triplets.

```cpp
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        sort(nums.begin(),nums.end());
        vector<vector<int>>res;
        for(int i=0;i<nums.size();i++){
            if(i>0 && nums[i]==nums[i-1])
            continue;
            int j=i+1,k=nums.size()-1;
            while(j<k){
                if(nums[i]+nums[j]+nums[k]==0)
                {
                    res.push_back({nums[i],nums[j],nums[k]});
                    while(j+1<k && nums[j]==nums[j+1])
                    j++;
                    j++;
                    while(k-1>j && nums[k]==nums[k-1])
                    k--;
                    k--;
                }
                else if(nums[i]+nums[j]+nums[k]>0)
                k--;
                else
                j++;
            }
        }
        return res;
    }
};
```

### 9 4Sum

Given an array nums of n integers, return an array of all the unique quadruplets [nums[a], nums[b], nums[c], nums[d]] such that:

0 <= a, b, c, d < n
a, b, c, and d are distinct.
nums[a] + nums[b] + nums[c] + nums[d] == target
You may return the answer in any order.

```cpp
class Solution {
public:
    vector<vector<int>> fourSum(vector<int>& nums, int target) {
        sort(nums.begin(),nums.end());
        vector<vector<int>>res;
        for(int i=0;i<nums.size();i++){
            if(i>0 && nums[i]==nums[i-1])
            continue;
            for(int j=i+1;j<nums.size();j++){
                if(j>i+1 && nums[j]==nums[j-1])
                continue;
                int k=j+1,t=nums.size()-1;
                while(k<t){
                    long long sum=(long long)nums[i]+nums[j]+nums[k]+nums[t];
                    if(sum==target)
                    {
                        res.push_back({nums[i],nums[j],nums[k],nums[t]});
                        while(k+1<t && nums[k]==nums[k+1])
                        k++;
                        k++;
                        while(t-1>k && nums[t]==nums[t-1])
                        t--;
                        t--;
                    }
                    else if(sum>target)
                    t--;
                    else
                    k++;
                }
            }
        }
        return res;
    }
};
```

### 10 Rotate Array

Given an integer array nums, rotate the array to the right by k steps, where k is non-negative.

```cpp
class Solution {
public:
    void rotate(vector<int>& nums, int k) {
        k%=nums.size();
        reverse(nums.begin(),nums.begin()+(nums.size()-k));
        reverse(nums.begin()+(nums.size()-k),nums.end());
        reverse(nums.begin(),nums.end());
    }
};
```

### 11 Container With Most Water

You are given an integer array height of length n. There are n vertical lines drawn such that the two endpoints of the ith line are (i, 0) and (i, height[i]).

Find two lines that together with the x-axis form a container, such that the container contains the most water.

Return the maximum amount of water a container can store.

Notice that you may not slant the container.

```cpp
class Solution {
public:
    int maxArea(vector<int>& heights) {
        int l=0,h=heights.size()-1,maxarea=0;
        while(l<h){
            maxarea=max(maxarea,min(heights[l],heights[h])*(h-l));
            if(heights[l]>heights[h])
            h--;
            else
            l++;
        }
        return maxarea;
    }
};
```

### 12 Boats to Save People

You are given an array people where people[i] is the weight of the ith person, and an infinite number of boats where each boat can carry a maximum weight of limit. Each boat carries at most two people at the same time, provided the sum of the weight of those people is at most limit.

Return the minimum number of boats to carry every given person.

```cpp
class Solution {
public:
    int numRescueBoats(vector<int>& people, int limit) {
        sort(people.begin(),people.end());
        int l=0,r=people.size()-1,cnt=0;
        while(l<=r){
            int rem=limit-people[r--];
            cnt++;
            if(l<=r && rem>=people[l])
            l++;
        }
        return cnt;
    }
};
```

### 13 Trapping Rain Water

Given n non-negative integers representing an elevation map where the width of each bar is 1, compute how much water it can trap after raining.

```cpp
class Solution {
public:
    int trap(vector<int>& height) {
        stack<int>stk;
        int vol=0;
        for(int i=0;i<height.size();i++){
            while(!stk.empty() && height[i]>=height[stk.top()]){
                int curr=stk.top();
                stk.pop();
                if(stk.empty())
                break;
                int len=i-stk.top()-1;
                vol+=(min(height[i],height[stk.top()])-height[curr])*len;
            }
            stk.push(i);
        }
        return vol;
    }
};
```
