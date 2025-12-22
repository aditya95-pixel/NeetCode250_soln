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

## 6 Design Twitter

Design a simplified version of Twitter where users can post tweets, follow/unfollow another user, and is able to see the 10 most recent tweets in the user's news feed.

Implement the Twitter class:

Twitter() Initializes your twitter object.
void postTweet(int userId, int tweetId) Composes a new tweet with ID tweetId by the user userId. Each call to this function will be made with a unique tweetId.
List<Integer> getNewsFeed(int userId) Retrieves the 10 most recent tweet IDs in the user's news feed. Each item in the news feed must be posted by users who the user followed or by the user themself. Tweets must be ordered from most recent to least recent.
void follow(int followerId, int followeeId) The user with ID followerId started following the user with ID followeeId.
void unfollow(int followerId, int followeeId) The user with ID followerId started unfollowing the user with ID followeeId.

```cpp
class Twitter {
    map<int,set<vector<int>>>mp;
    map<int,set<int>>followees;
    int time=1;
public:
    Twitter() {
        
    }
    
    void postTweet(int userId, int tweetId) {
        mp[userId].insert({time++,tweetId});
    }
    
    vector<int> getNewsFeed(int userId) {
        int cnt=10;
        priority_queue<vector<int>>pq;
        vector<int>res;
        for(auto item:mp[userId])
        pq.push(item);
        for(auto f:followees[userId])
        {
            for(auto item:mp[f])
            pq.push(item);
        }
        while(cnt && !pq.empty()){
            int twtId=pq.top()[1];
            pq.pop();
            cnt--;
            res.push_back(twtId);
        }
        return res;
    }
    
    void follow(int followerId, int followeeId) {
        followees[followerId].insert(followeeId);
    }
    
    void unfollow(int followerId, int followeeId) {
        followees[followerId].erase(followeeId);
    }
};
```

## 7 Single-Threaded CPU

You are given n​​​​​​ tasks labeled from 0 to n - 1 represented by a 2D integer array tasks, where tasks[i] = [enqueueTimei, processingTimei] means that the i​​​​​​th​​​​ task will be available to process at enqueueTimei and will take processingTimei to finish processing.

You have a single-threaded CPU that can process at most one task at a time and will act in the following way:

If the CPU is idle and there are no available tasks to process, the CPU remains idle.
If the CPU is idle and there are available tasks, the CPU will choose the one with the shortest processing time. If multiple tasks have the same shortest processing time, it will choose the task with the smallest index.
Once a task is started, the CPU will process the entire task without stopping.
The CPU can finish a task then start a new one instantly.
Return the order in which the CPU will process the tasks.

```cpp
class Solution {
public:
    static bool cmp(vector<int>&a,vector<int>&b){
        if(a[0]<b[0])
        return 1;
        else if(a[0]>b[0])
        return 0;
        else
        return (a[1]<b[1]);
    }
    vector<int> getOrder(vector<vector<int>>& tasks) {
        vector<vector<int>>tsks;
        for(int i=0;i<tasks.size();i++){
            vector<int> task=tasks[i];
            tsks.push_back({task[0],task[1],i});
        }
        sort(tsks.begin(),tsks.end(),cmp);
        priority_queue<pair<int,int>,vector<pair<int,int>>,greater<>>pq;
        long long time=tsks[0][0];
        int i=0;
        while(i<tsks.size() && time==tsks[i][0]){
            pq.push({tsks[i][1],tsks[i][2]});
            i++;
        }
        vector<int>res;
        while(i<tsks.size()){
            if(!pq.empty()){
                int dur=pq.top().first,idx=pq.top().second;
                pq.pop();
                time+=dur;
                res.push_back(idx);
                while(i<tsks.size() && time>=tsks[i][0])
                {
                    pq.push({tsks[i][1],tsks[i][2]});
                    i++;
                }
            }else{
                time=tsks[i][0];
                while(i<tsks.size() && time==tsks[i][0]){
                    pq.push({tsks[i][1],tsks[i][2]});
                    i++;
                }
            }
        }
        while(!pq.empty()){
            int dur=pq.top().first,idx=pq.top().second;
            pq.pop();
            time+=dur;
            res.push_back(idx);
        }
        return res;
    }
};
```
