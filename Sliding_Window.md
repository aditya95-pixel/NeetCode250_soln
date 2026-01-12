### 1 Contains Duplicate II

Given an integer array nums and an integer k, return true if there are two distinct indices i and j in the array such that nums[i] == nums[j] and abs(i - j) <= k.

```cpp
class Solution {
public:
    bool containsNearbyDuplicate(vector<int>& nums, int k) {
        unordered_map<int,int>mp;
        for(int i=0;i<nums.size();i++){
            if(mp.count(nums[i]) && i-mp[nums[i]]<=k)
            return 1;
            mp[nums[i]]=i;
        }
        return 0;
    }
};
```

### 2 Best Time to Buy and Sell Stock

You are given an array prices where prices[i] is the price of a given stock on the ith day.

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int min_till_now=INT_MAX,maxp=0;
        for(int i=0;i<prices.size();i++){
            min_till_now=min(min_till_now,prices[i]);
            maxp=max(maxp,prices[i]-min_till_now);
        }
        return maxp;
    }
};
```

### 3 Longest Substring Without Repeating Characters

Given a string s, find the length of the longest substring without duplicate characters.

```cpp
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        unordered_map<int,int>mp;
        int j=0,maxlen=0;
        for(int i=0;i<s.size();i++){
            if(mp.count(s[i])){
                while(s[j]!=s[i])
                mp.erase(s[j++]);
                mp.erase(s[j++]);
            }
            maxlen=max(maxlen,i-j+1);
            mp[s[i]]++;
        }
        return maxlen;
    }
};
```

### 4 Longest Repeating Character Replacement

You are given a string s and an integer k. You can choose any character of the string and change it to any other uppercase English character. You can perform this operation at most k times.

Return the length of the longest substring containing the same letter you can get after performing the above operations.

```cpp
class Solution {
public:
    int characterReplacement(string s, int k) {
        int l=0,maxlen=0,maxf=0;
        unordered_map<int,int>mp;
        for(int r=0;r<s.size();r++){
            mp[s[r]]++;
            maxf=max(maxf,mp[s[r]]);
            while((r-l+1)-maxf>k){
                mp[s[l]]--;
                l++;
            }
            maxlen=max(maxlen,r-l+1);
        }
        return maxlen;
    }
};
```

### 5 Permutation in String

Given two strings s1 and s2, return true if s2 contains a permutation of s1, or false otherwise.

In other words, return true if one of s1's permutations is the substring of s2.

```cpp
class Solution {
public:
    bool checkInclusion(string s1, string s2) {
        if(s1.size()>s2.size())
        return false;
        unordered_map<char,int>mp1,mp2;
        for(int i=0;i<s1.size();i++)
        mp1[s1[i]]++;
        for(int i=0;i<s1.size();i++)
        mp2[s2[i]]++;
        int matches=0;
        for(char c='a';c<='z';c++){
            if(mp1[c]==mp2[c])
            matches++;
        }
        int l=0;
        for(int r=s1.size();r<s2.size();r++){
            if(matches==26)
            return 1;
            mp2[s2[r]]++;
            if(mp2[s2[r]]==mp1[s2[r]])
            matches++;
            else if(mp2[s2[r]]==mp1[s2[r]]+1)
            matches--;
            mp2[s2[l]]--;
            if(mp2[s2[l]]==mp1[s2[l]])
            matches++;
            else if(mp2[s2[l]]+1==mp1[s2[l]])
            matches--;
            l++;
        }
        return matches==26;
    }
};
```

### 6 Minimum Size Subarray Sum

Given an array of positive integers nums and a positive integer target, return the minimal length of a subarray whose sum is greater than or equal to target. If there is no such subarray, return 0 instead.
You want to maximize your profit by choosing a single day to buy one stock and choosing a different day in the future to sell that stock.

Return the maximum profit you can achieve from this transaction. If you cannot achieve any profit, return 0.

```cpp
class Solution {
public:
    int minSubArrayLen(int target, vector<int>& nums) {
        if(accumulate(nums.begin(),nums.end(),0)<target)
        return 0;
        int l=0,r=0,minlen=INT_MAX,sum=0;
        while(r<nums.size()){
            sum+=nums[r];
            while(sum>=target){
                minlen=min(minlen,r-l+1);
                sum-=nums[l++];
            }
            r++;
        }
        return minlen;
    }
};
```

### 7 Find K Closest Elements

Given a sorted integer array arr, two integers k and x, return the k closest integers to x in the array. The result should also be sorted in ascending order.

An integer a is closer to x than an integer b if:

|a - x| < |b - x|, or
|a - x| == |b - x| and a < b

```cpp
class Solution {
public:
    vector<int> findClosestElements(vector<int>& arr, int k, int x) {
        int l=0,r=arr.size()-1;
        while(r-l>=k){
            if(abs(x-arr[l])>abs(x-arr[r]))
            l++;
            else
            r--;
        }
        return vector<int>(arr.begin()+l,arr.begin()+r+1);
    }
};
```

### 8 Minimum Window Substring

Given two strings s and t of lengths m and n respectively, return the minimum window substring of s such that every character in t (including duplicates) is included in the window. If there is no such substring, return the empty string "".

The testcases will be generated such that the answer is unique.

```cpp
class Solution {
public:
    string minWindow(string s, string t) {
        unordered_map<char,int>mp1,mp2;
        for(auto &c:t)
        mp1[c]++;
        int l=0,r=0,minlen=INT_MAX,need=0;
        int begin=-1,end=-1;
        while(r<s.size()){
            mp2[s[r]]++;
            if(mp1.count(s[r]) && mp2[s[r]]==mp1[s[r]])
            need++;
            while(need==mp1.size()){
                if(minlen>r-l+1){
                    minlen=r-l+1;
                    begin=l;
                    end=r;
                }
                mp2[s[l]]--;
                if(mp1.count(s[l]) && mp2[s[l]]+1==mp1[s[l]])
                need--;
                l++;
            }
            r++;
        }
        if(minlen==INT_MAX)
        return "";
        else
        return s.substr(begin,minlen);
    }
};
```

### 9 Sliding Window Maximum

You are given an array of integers nums, there is a sliding window of size k which is moving from the very left of the array to the very right. You can only see the k numbers in the window. Each time the sliding window moves right by one position.

Return the max sliding window.

```cpp
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        vector<int>res;
        deque<pair<int,int>>q;
        for(int i=0;i<k;i++){
            while(!q.empty() && q.back().first<=nums[i])
                q.pop_back();
            q.push_back({nums[i],i});
        }
        res.push_back(q.front().first);
        for(int i=k;i<nums.size();i++){
            while(!q.empty() && q.front().second<=i-k)
                q.pop_front();
            while(!q.empty() && q.back().first<=nums[i])
                q.pop_back();
            q.push_back({nums[i],i});
            res.push_back(q.front().first);
        }
        return res;
    }
};
```
