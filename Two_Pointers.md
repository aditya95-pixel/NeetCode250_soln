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
