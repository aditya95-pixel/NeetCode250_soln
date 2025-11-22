### 1 Concatenation of Array
You are given an integer array nums of length n. Create an array ans of length 2n where ans[i] == nums[i] and ans[i + n] == nums[i] for 0 <= i < n (0-indexed).

Specifically, ans is the concatenation of two nums arrays.

Return the array ans.

```cpp
class Solution {
public:
    vector<int> getConcatenation(vector<int>& nums) {
        vector<int>res(nums.size()*2);
        for(int i=0;i<nums.size();i++)
        res[i]=res[nums.size()+i]=nums[i];
        return res;
    }
};
```

### 2 Contains Duplicate

Given an integer array nums, return true if any value appears more than once in the array, otherwise return false.

```cpp
class Solution {
public:
    bool containsDuplicate(vector<int>& nums) {
        set<int>s;
        for(auto &num:nums){
            if(s.count(num))
            return 1;
            s.insert(num);
        }
        return 0;
    }
};
```

### 3 Valid Anagram

Given two strings s and t, return true if the two strings are anagrams of each other, otherwise return false.

An anagram is a string that contains the exact same characters as another string, but the order of the characters can be different.

```cpp
class Solution {
public:
    bool isAnagram(string s, string t) {
        if(s.size()!=t.size())
        return 0;
        unordered_map<char,int>mp1,mp2;
        for(int i=0;i<s.size();i++)
        {
            mp1[s[i]]++;
            mp2[t[i]]++;
        }
        return mp1==mp2;
    }
};
```

### 4 Two Sum
Given an array of integers nums and an integer target, return the indices i and j such that nums[i] + nums[j] == target and i != j.

You may assume that every input has exactly one pair of indices i and j that satisfy the condition.

Return the answer with the smaller index first.

```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int,int>mp;
        vector<int>res;
        for(int i=0;i<nums.size();i++){
            if(mp.count(target-nums[i]))
            {
                res.push_back(mp[target-nums[i]]);
                res.push_back(i);
                break;
            }
            mp[nums[i]]=i;
        }
        return res;
    }
};
```
