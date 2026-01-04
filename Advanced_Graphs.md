## 1 Path With Minimum Effort

You are a hiker preparing for an upcoming hike. You are given heights, a 2D array of size rows x columns, where heights[row][col] represents the height of cell (row, col). You are situated in the top-left cell, (0, 0), and you hope to travel to the bottom-right cell, (rows-1, columns-1) (i.e., 0-indexed). You can move up, down, left, or right, and you wish to find a route that requires the minimum effort.

A route's effort is the maximum absolute difference in heights between two consecutive cells of the route.

Return the minimum effort required to travel from the top-left cell to the bottom-right cell.

```cpp
class Solution {
public:
    int minimumEffortPath(vector<vector<int>>& heights) {
        int cnt=heights.size()*heights[0].size();
        vector<vector<pair<int,int>>>adj(cnt);
        for(int i=0;i<heights.size();i++){
            for(int j=0;j<heights[0].size();j++){
                int node=i*heights[0].size()+j;
                if(i-1>=0)
                {
                    int new_node=(i-1)*heights[0].size()+j;
                    int d=abs(heights[i][j]-heights[i-1][j]);
                    adj[node].push_back({new_node,d});
                }
                if(i+1<heights.size())
                {
                    int new_node=(i+1)*heights[0].size()+j;
                    int d=abs(heights[i][j]-heights[i+1][j]);
                    adj[node].push_back({new_node,d});
                }
                if(j-1>=0)
                {
                    int new_node=i*heights[0].size()+j-1;
                    int d=abs(heights[i][j]-heights[i][j-1]);
                    adj[node].push_back({new_node,d});
                }
                if(j+1<heights[0].size())
                {
                    int new_node=i*heights[0].size()+j+1;
                    int d=abs(heights[i][j]-heights[i][j+1]);
                    adj[node].push_back({new_node,d});
                }
            }
        }
        vector<int>dist(cnt,INT_MAX);
        dist[0]=0;
        priority_queue<pair<int,int>,vector<pair<int,int>>,greater<>>pq;
        pq.push({0,0});
        while(!pq.empty()){
            int u=pq.top().second,d1=pq.top().first;
            pq.pop();
            for(auto &item:adj[u]){
                int v=item.first,d2=item.second;
                int d=max(d1,d2);
                if(dist[v]>d)
                {
                    dist[v]=d;
                    pq.push({dist[v],v});
                }
            }
        }
        return dist[heights.size()*heights[0].size()-1];
    }
};
```

## 2 Network Delay Time

You are given a network of n nodes, labeled from 1 to n. You are also given times, a list of travel times as directed edges times[i] = (ui, vi, wi), where ui is the source node, vi is the target node, and wi is the time it takes for a signal to travel from source to target.

We will send a signal from a given node k. Return the minimum time it takes for all the n nodes to receive the signal. If it is impossible for all the n nodes to receive the signal, return -1.

```cpp
class Solution {
public:
    int networkDelayTime(vector<vector<int>>& times, int n, int k) {
        vector<int>dist(n,INT_MAX);
        dist[k-1]=0;
        vector<vector<pair<int,int>>>adj(n);
        for(auto &time:times){
            int u=time[0],v=time[1],d=time[2];
            adj[u-1].push_back({v-1,d});
        }
        priority_queue<pair<int,int>,vector<pair<int,int>>,greater<>>pq;
        pq.push({0,k-1});
        while(!pq.empty()){
            int u=pq.top().second;
            pq.pop();
            for(auto &item:adj[u]){
                int v=item.first,d=item.second;
                if(dist[v]>dist[u]+d){
                    dist[v]=dist[u]+d;
                    pq.push({d,v});
                }
            }
        }
        int maxo=*max_element(dist.begin(),dist.end());
        return (maxo==INT_MAX)?-1:maxo;
    }
};
```
