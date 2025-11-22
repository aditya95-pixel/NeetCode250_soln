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
