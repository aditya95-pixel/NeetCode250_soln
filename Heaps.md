## 1 Kth Largest Element in a Stream

You are part of a university admissions office and need to keep track of the kth highest test score from applicants in real-time. This helps to determine cut-off marks for interviews and admissions dynamically as new applicants submit their scores.

You are tasked to implement a class which, for a given integer k, maintains a stream of test scores and continuously returns the kth highest test score after a new score has been submitted. More specifically, we are looking for the kth highest score in the sorted list of all scores.

Implement the KthLargest class:

KthLargest(int k, int[] nums) Initializes the object with the integer k and the stream of test scores nums.
int add(int val) Adds a new test score val to the stream and returns the element representing the kth largest element in the pool of test scores so far.

```cpp
class KthLargest {
    priority_queue<int,vector<int>,greater<>>pq;
    int k;
public:
    KthLargest(int k, vector<int>& nums) {
        this->k=k;
        int k_=k,i=nums.size()-1;
        sort(nums.begin(),nums.end());
        while(k_-- && i>=0)
        pq.push(nums[i--]);
    }
    
    int add(int val) {
        if(pq.size()<k)
            pq.push(val);
        else if(val>pq.top() && pq.size()==k)
        {
            pq.pop();
            pq.push(val);
        }
        return pq.top();
    }
};
```

## Last Stone Weight

You are given an array of integers stones where stones[i] is the weight of the ith stone.

We are playing a game with the stones. On each turn, we choose the heaviest two stones and smash them together. Suppose the heaviest two stones have weights x and y with x <= y. The result of this smash is:

If x == y, both stones are destroyed, and
If x != y, the stone of weight x is destroyed, and the stone of weight y has new weight y - x.
At the end of the game, there is at most one stone left.

Return the weight of the last remaining stone. If there are no stones left, return 0.

```cpp
class Solution {
public:
    int lastStoneWeight(vector<int>& stones) {
        priority_queue<int>pq;
        for(auto &stone:stones)
        pq.push(stone);
        while(pq.size()>1){
            int stone1=pq.top();
            pq.pop();
            int stone2=pq.top();
            pq.pop();
            if(stone1!=stone2)
            pq.push(abs(stone1-stone2));
        }
        return pq.empty()?0:pq.top();
    }
};
```

## 3 K Closest Points to Origin

Given an array of points where points[i] = [xi, yi] represents a point on the X-Y plane and an integer k, return the k closest points to the origin (0, 0).

The distance between two points on the X-Y plane is the Euclidean distance (i.e., √(x1 - x2)2 + (y1 - y2)2).

You may return the answer in any order. The answer is guaranteed to be unique (except for the order that it is in).

```cpp
class Solution {
public:
    vector<vector<int>> kClosest(vector<vector<int>>& points, int k) {
        vector<vector<int>>res;
        priority_queue<pair<int,pair<int,int>>,vector<pair<int,pair<int,int>>>,greater<pair<int,pair<int,int>>>>pq;
        for(auto point:points)
            pq.push({point[0]*point[0]+point[1]*point[1],{point[0],point[1]}});
        while(k--){
            int x=pq.top().second.first,y=pq.top().second.second;
            pq.pop();
            res.push_back({x,y});
        }
        return res;
    }
};
```

## 4 Kth Largest Element in an Array

Given an integer array nums and an integer k, return the kth largest element in the array.

Note that it is the kth largest element in the sorted order, not the kth distinct element.

Can you solve it without sorting?

```cpp
class Solution {
public:
    int findKthLargest(vector<int>& nums, int k) {
        priority_queue<int,vector<int>>pq;
        for(auto &ele:nums)
        pq.push(ele);
        while(k-->1)
        pq.pop();
        return pq.top();
    }
};
```

## 5 Task Scheduler

You are given an array of CPU tasks, each labeled with a letter from A to Z, and a number n. Each CPU interval can be idle or allow the completion of one task. Tasks can be completed in any order, but there's a constraint: there has to be a gap of at least n intervals between two tasks with the same label.

Return the minimum number of CPU intervals required to complete all tasks.

```cpp
class Solution {
public:
    int leastInterval(vector<char>& tasks, int n) {
        vector<int>freq(26,0);
        for(auto task:tasks)
        freq[task-'A']++;
        sort(freq.rbegin(),freq.rend());
        int maxo=freq[0];
        int idle=(maxo-1)*n;
        for(int i=1;i<26;i++)
        {
            int sub=min(maxo-1,freq[i]);
            if(sub>idle)
            {
                idle=0;
                break;
            }else
            idle-=sub;
        }
        return idle+tasks.size();
    }
};
```

## 
