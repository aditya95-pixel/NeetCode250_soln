## 1 Insert Interval

You are given an array of non-overlapping intervals intervals where intervals[i] = [starti, endi] represent the start and the end of the ith interval and intervals is sorted in ascending order by starti. You are also given an interval newInterval = [start, end] that represents the start and end of another interval.

Insert newInterval into intervals such that intervals is still sorted in ascending order by starti and intervals still does not have any overlapping intervals (merge overlapping intervals if necessary).

Return intervals after the insertion.

Note that you don't need to modify intervals in-place. You can make a new array and return it.

```cpp
class Solution {
public:
    vector<vector<int>> insert(vector<vector<int>>& intervals, vector<int>& newInterval) {
        intervals.push_back(newInterval);
        sort(intervals.begin(),intervals.end());
        vector<vector<int>>res;
        res.push_back(intervals[0]);
        for(int i=1;i<intervals.size();i++){
            vector<int>&last=res.back();
            if(intervals[i][0]<=last[1])
                last[1]=max(last[1],intervals[i][1]);
            else
                res.push_back(intervals[i]);
        }
        return res;
    }
};
```

## 2 Merge Intervals

Given an array of intervals where intervals[i] = [starti, endi], merge all overlapping intervals, and return an array of the non-overlapping intervals that cover all the intervals in the input.

```cpp
class Solution {
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        sort(intervals.begin(),intervals.end());
        vector<vector<int>>res;
        res.push_back(intervals[0]);
        for(int i=1;i<intervals.size();i++){
            vector<int>&last=res.back();
            if(intervals[i][0]<=last[1])
            last[1]=max(last[1],intervals[i][1]);
            else
            res.push_back(intervals[i]);
        }
        return res;
    }
};
```

## 3 Non-overlapping Intervals

Given an array of intervals intervals where intervals[i] = [starti, endi], return the minimum number of intervals you need to remove to make the rest of the intervals non-overlapping.

Note that intervals which only touch at a point are non-overlapping. For example, [1, 2] and [2, 3] are non-overlapping.

```cpp
class Solution {
public:
    static bool cmp(vector<int>&a,vector<int>&b){
        return a[1]<b[1];
    }
    int eraseOverlapIntervals(vector<vector<int>>& intervals) {
        sort(intervals.begin(),intervals.end(),cmp);
        int cnt=0,last=intervals[0][1];
        for(int i=1;i<intervals.size();i++){
            if(intervals[i][0]>=last)
            last=intervals[i][1];
            else
            cnt++;
        }
        return cnt;
    }
};
```

## 4 Meeting Rooms

Given an array of meeting time interval objects consisting of start and end times [[start_1,end_1],[start_2,end_2],...] (start_i < end_i), determine if a person could add all meetings to their schedule without any conflicts.

Note: (0,8),(8,10) is not considered a conflict at 8

```cpp
class Solution {
public:
    static bool cmp(Interval &a,Interval &b){
        return a.end<b.end;
    }
    bool canAttendMeetings(vector<Interval>& intervals) {
        if(intervals.empty())
        return 1;
        sort(intervals.begin(),intervals.end(),cmp);
        int last=intervals[0].end;
        for(int i=1;i<intervals.size();i++){
            if(intervals[i].start>=last)
            last=intervals[i].end;
            else
            return 0;
        }
        return 1;
    }
};
```

## 5 Meeting Rooms II

Given an array of meeting time interval objects consisting of start and end times [[start_1,end_1],[start_2,end_2],...] (start_i < end_i), find the minimum number of days required to schedule all meetings without any conflicts.

Note: (0,8),(8,10) is not considered a conflict at 8.

```cpp
class Solution {
public:
    int minMeetingRooms(vector<Interval>& intervals) {
        vector<int>starts,ends;
        for(auto &item:intervals){
            starts.push_back(item.start);
            ends.push_back(item.end);
        }
        sort(starts.begin(),starts.end());
        sort(ends.begin(),ends.end());
        int i=0,j=0,cnt=0,res=0;
        while(i<starts.size() && j<ends.size()){
            if(starts[i]<ends[j]){
                i++;
                cnt++;
            }else{
                j++;
                cnt--;
            }
            res=max(res,cnt);
        }
        return res;
    }
};
```

## 6 Meeting Rooms III

You are given an integer n. There are n rooms numbered from 0 to n - 1.

You are given a 2D integer array meetings where meetings[i] = [starti, endi] means that a meeting will be held during the half-closed time interval [starti, endi). All the values of starti are unique.

Meetings are allocated to rooms in the following manner:

Each meeting will take place in the unused room with the lowest number.
If there are no available rooms, the meeting will be delayed until a room becomes free. The delayed meeting should have the same duration as the original meeting.
When a room becomes unused, meetings that have an earlier original start time should be given the room.
Return the number of the room that held the most meetings. If there are multiple rooms, return the room with the lowest number.

A half-closed interval [a, b) is the interval between a and b including a and not including b.

```cpp
class Solution {
public:
    int mostBooked(int n, vector<vector<int>>& meetings) {
        sort(meetings.begin(),meetings.end());
        priority_queue<int,vector<int>,greater<>>avail;
        priority_queue<pair<long long,int>,vector<pair<long long,int>>,greater<>>occ;
        vector<int>freq(n,0);
        for(int i=0;i<n;i++)
        avail.push(i);
        long long time=0;
        for(int i=0;i<meetings.size();i++){
            time=max(time,1LL*meetings[i][0]);
            while(!occ.empty() && time>=occ.top().first)
            {
                int room=occ.top().second;
                occ.pop();
                avail.push(room);
            }
            if(!avail.empty()){
                int room=avail.top();
                avail.pop();
                occ.push({time+meetings[i][1]-meetings[i][0],room});
                freq[room]++;
            }else{
                time=occ.top().first;
                int room=occ.top().second;
                occ.pop();
                occ.push({time+meetings[i][1]-meetings[i][0],room});
                freq[room]++;
            }
        }
        int maxo=*max_element(freq.begin(),freq.end());
        for(int i=0;i<n;i++){
            if(freq[i]==maxo)
            return i;
        }
        return -1;
    }
};
```

## 7 Minimum Interval to Include Each Query

You are given a 2D integer array intervals, where intervals[i] = [lefti, righti] describes the ith interval starting at lefti and ending at righti (inclusive). The size of an interval is defined as the number of integers it contains, or more formally righti - lefti + 1.

You are also given an integer array queries. The answer to the jth query is the size of the smallest interval i such that lefti <= queries[j] <= righti. If no such interval exists, the answer is -1.

Return an array containing the answers to the queries.

```cpp
class Solution {
public:
    vector<int> minInterval(vector<vector<int>>& intervals, vector<int>& queries) {
        sort(intervals.begin(),intervals.end());
        vector<pair<int,int>>vp;
        for(int i=0;i<queries.size();i++)
            vp.push_back({queries[i],i});
        sort(vp.begin(),vp.end());
        vector<int>res(queries.size(),-1);
        priority_queue<pair<int,int>,vector<pair<int,int>>,greater<>>pq;
        int i=0;
        for(auto &item:vp){
            int q=item.first,idx=item.second;
            while(i<intervals.size() && intervals[i][0]<=q)
            pq.push({intervals[i][1]-intervals[i][0]+1,intervals[i++][1]});
            while(!pq.empty() && pq.top().second<q)
            pq.pop();
            if(!pq.empty())
            res[idx]=pq.top().first;
        }
        return res;
    }
};
```
