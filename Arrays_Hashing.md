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

### 5 Longest Common Prefix

You are given an array of strings strs. Return the longest common prefix of all the strings.

If there is no longest common prefix, return an empty string "".

```cpp
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
        string base=strs[0];
        for(int i=1;i<strs.size();i++){
            string &str=strs[i];
            int j=0;
            while(j<base.size() && j<str.size() && str[j]==base[j])
            j++;
            string pref=base.substr(0,j);
            base=pref;
        }
        return base;
    }
};
```

### 6 Group Anagrams

Given an array of strings strs, group all anagrams together into sublists. You may return the output in any order.

An anagram is a string that contains the exact same characters as another string, but the order of the characters can be different.

```cpp
class Solution {
public:
    string hash(string s){
        vector<int>freq(26,0);
        for(auto &c:s)
        freq[c-'a']++;
        string h;
        for(auto &val:freq)
        h+=to_string(val)+" ";
        return h;
    }
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        vector<vector<string>>res;
        unordered_map<string,vector<string>>mp;
        for(auto &str:strs){
            string h=hash(str);
            mp[h].push_back(str);
        }
        for(auto &item:mp)
        res.push_back(item.second);
        return res;
    }
};
```
