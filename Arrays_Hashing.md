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

### 7 Remove Element

Given an integer array nums and an integer val, remove all occurrences of val in nums in-place. The order of the elements may be changed. Then return the number of elements in nums which are not equal to val.

Consider the number of elements in nums which are not equal to val be k, to get accepted, you need to do the following things:

Change the array nums such that the first k elements of nums contain the elements which are not equal to val. The remaining elements of nums are not important as well as the size of nums.
Return k.

```cpp
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int k=0;
        for(int i=0;i<nums.size();i++){
            if(nums[i]!=val)
            swap(nums[i],nums[k++]);
        }
        return k;
    }
};
```

### 8 Majority Element

Given an array nums of size n, return the majority element.

The majority element is the element that appears more than ⌊n / 2⌋ times. You may assume that the majority element always exists in the array.

```cpp
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        unordered_map<int,int>mp;
        int n=nums.size();
        for(auto &num:nums){
            mp[num]++;
            if(mp[num]>n/2)
            return num;
        }
        return -1;
    }
};
```

### 9 Design HashSet

Design a HashSet without using any built-in hash table libraries.

Implement MyHashSet class:

void add(key) Inserts the value key into the HashSet.
bool contains(key) Returns whether the value key exists in the HashSet or not.
void remove(key) Removes the value key in the HashSet. If key does not exist in the HashSet, do nothing.

```cpp
class MyHashSet {
    vector<int>v;
public:
    MyHashSet() {
        v.resize(1000001,0);
    }
    
    void add(int key) {
        v[key]=1;
    }
    
    void remove(int key) {
        v[key]=0;
    }
    
    bool contains(int key) {
        return v[key];
    }
};

/**
 * Your MyHashSet object will be instantiated and called as such:
 * MyHashSet* obj = new MyHashSet();
 * obj->add(key);
 * obj->remove(key);
 * bool param_3 = obj->contains(key);
 */
 ```

### 10 Design HashMap

Design a HashMap without using any built-in hash table libraries.

Implement the MyHashMap class:

MyHashMap() initializes the object with an empty map.
void put(int key, int value) inserts a (key, value) pair into the HashMap. If the key already exists in the map, update the corresponding value.
int get(int key) returns the value to which the specified key is mapped, or -1 if this map contains no mapping for the key.
void remove(key) removes the key and its corresponding value if the map contains the mapping for the key.

```cpp
class MyHashMap {
    vector<int>v;
public:
    MyHashMap() {
        v.resize(1000001,-1);
    }
    
    void put(int key, int value) {
        v[key]=value;
    }
    
    int get(int key) {
        return v[key];
    }
    
    void remove(int key) {
        v[key]=-1;
    }
};

/**
 * Your MyHashMap object will be instantiated and called as such:
 * MyHashMap* obj = new MyHashMap();
 * obj->put(key,value);
 * int param_2 = obj->get(key);
 * obj->remove(key);
 */
 ```

### 11 Sort an Array

Given an array of integers nums, sort the array in ascending order and return it.

You must solve the problem without using any built-in functions in O(nlog(n)) time complexity and with the smallest space complexity possible.

```cpp
class Solution {
public:
    void merge(vector<int>&nums,int l,int mid,int r){
        int n1=mid-l+1,n2=r-mid;
        vector<int>nums1(n1),nums2(n2);
        for(int i=0;i<n1;i++)
        nums1[i]=nums[l+i];
        for(int i=0;i<n2;i++)
        nums2[i]=nums[mid+i+1];
        int k=l,i=0,j=0;
        while(i<nums1.size() && j<nums2.size()){
            if(nums1[i]<nums2[j])
            {
                nums[k++]=nums1[i++];
            }else{
                nums[k++]=nums2[j++];
            }
        }
        while(i<nums1.size())
        nums[k++]=nums1[i++];
        while(j<nums2.size())
        nums[k++]=nums2[j++];
    }
    void mergesort(vector<int>&nums,int l,int r){
        if(l<r){
            int mid=l+(r-l)/2;
            mergesort(nums,l,mid);
            mergesort(nums,mid+1,r);
            merge(nums,l,mid,r); 
        }
    }
    vector<int> sortArray(vector<int>& nums) {
        mergesort(nums,0,nums.size()-1);
        return nums;
    }
};
```
