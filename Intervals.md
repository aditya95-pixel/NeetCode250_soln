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
