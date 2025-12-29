## 1 Lemonade Change

At a lemonade stand, each lemonade costs $5. Customers are standing in a queue to buy from you and order one at a time (in the order specified by bills). Each customer will only buy one lemonade and pay with either a $5, $10, or $20 bill. You must provide the correct change to each customer so that the net transaction is that the customer pays $5.

Note that you do not have any change in hand at first.

Given an integer array bills where bills[i] is the bill the ith customer pays, return true if you can provide every customer with the correct change, or false otherwise.

```cpp
class Solution {
public:
    bool lemonadeChange(vector<int>& bills) {
        int curr5=0,curr10=0;
        for(int i=0;i<bills.size();i++){
            if(bills[i]==5)
            curr5++;
            else if(bills[i]==10)
            {
                if(curr5==0)
                return 0;
                curr5--;
                curr10++;
            }
            else if(bills[i]==20){
                if(curr10>0 && curr5>0)
                {
                    curr5--;
                    curr10--;
                }else if(curr5>2)
                    curr5-=3;
                else
                return 0;
            }
        }
        return 1;
    }
};
```

## 2 Maximum Subarray

Given an integer array nums, find the subarray with the largest sum, and return its sum.

```cpp
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int maxsum=nums[0],sum=nums[0];
        for(int i=1;i<nums.size();i++){
            sum=max(nums[i],sum+nums[i]);
            maxsum=max(maxsum,sum);
        }
        return maxsum;
    }
};
```

## 3 Maximum Sum Circular Subarray

Given a circular integer array nums of length n, return the maximum possible sum of a non-empty subarray of nums.

A circular array means the end of the array connects to the beginning of the array. Formally, the next element of nums[i] is nums[(i + 1) % n] and the previous element of nums[i] is nums[(i - 1 + n) % n].

A subarray may only include each element of the fixed buffer nums at most once. Formally, for a subarray nums[i], nums[i + 1], ..., nums[j], there does not exist i <= k1, k2 <= j with k1 % n == k2 % n.

```cpp
class Solution {
public:
    int maxSubarraySumCircular(vector<int>& nums) {
        int maxsum=nums[0],minsum=nums[0],sum1=nums[0],sum2=nums[0],totalsum=nums[0];
        for(int i=1;i<nums.size();i++){
            int num=nums[i];
            sum1=max(num,sum1+num);
            maxsum=max(maxsum,sum1);
            sum2=min(num,sum2+num);
            minsum=min(minsum,sum2);
            totalsum+=num;
        }
        if(totalsum==minsum)
            return maxsum;
        else
            return max(maxsum,totalsum-minsum);
    }
};
```
