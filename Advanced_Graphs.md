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
                    pq.push({dist[v],v});
                }
            }
        }
        int maxo=*max_element(dist.begin(),dist.end());
        return (maxo==INT_MAX)?-1:maxo;
    }
};
```

## 3 Reconstruct Itinerary

You are given a list of airline tickets where tickets[i] = [fromi, toi] represent the departure and the arrival airports of one flight. Reconstruct the itinerary in order and return it.

All of the tickets belong to a man who departs from "JFK", thus, the itinerary must begin with "JFK". If there are multiple valid itineraries, you should return the itinerary that has the smallest lexical order when read as a single string.

For example, the itinerary ["JFK", "LGA"] has a smaller lexical order than ["JFK", "LGB"].
You may assume all tickets form at least one valid itinerary. You must use all the tickets once and only once.

```cpp
class Solution {
public:
    void solve(string src,map<string,priority_queue<string,vector<string>,greater<>>>&mp,vector<string>&res){
        priority_queue<string,vector<string>,greater<>>&pq=mp[src];
        while(!pq.empty()){
            string dest=pq.top();
            pq.pop();
            solve(dest,mp,res);
        }
        res.push_back(src);
    }
    vector<string> findItinerary(vector<vector<string>>& tickets) {
        map<string,priority_queue<string,vector<string>,greater<>>>mp;
        for(auto &ticket:tickets)
            mp[ticket[0]].push(ticket[1]);
        vector<string>res;
        solve("JFK",mp,res);
        reverse(res.begin(),res.end());
        return res;
    }
};
```

## 4 Min Cost to Connect All Points

You are given an array points representing integer coordinates of some points on a 2D-plane, where points[i] = [xi, yi].

The cost of connecting two points [xi, yi] and [xj, yj] is the manhattan distance between them: |xi - xj| + |yi - yj|, where |val| denotes the absolute value of val.

Return the minimum cost to make all points connected. All points are connected if there is exactly one simple path between any two points.

```cpp
class DSU{
    vector<int>parent,rank;
    public:
    DSU(int n){
        parent.resize(n);
        rank.resize(n,0);
        iota(parent.begin(),parent.end(),0);
    }
    int find(int u){
        if(parent[u]==u)
        return u;
        return parent[u]=find(parent[u]);
    }
    void merge(int u,int v){
        u=find(u);
        v=find(v);
        if(rank[u]>rank[v])
            parent[v]=u;
        else if(rank[v]>rank[u])
            parent[u]=v;
        else{
            parent[v]=u;
            rank[u]++;
        }
    }
};
class Solution {
public:
    static bool cmp(vector<int>&a,vector<int>&b){
        return a[2]<b[2];
    }
    int minCostConnectPoints(vector<vector<int>>& points) {
        vector<vector<int>>edges;
        for(int i=0;i<points.size();i++){
            for(int j=i+1;j<points.size();j++)
            edges.push_back({i,j,abs(points[i][0]-points[j][0])+abs(points[i][1]-points[j][1])});
        }
        sort(edges.begin(),edges.end(),cmp);
        int cost=0,cnt=0;
        DSU d(points.size());
        for(auto &edge:edges){
            int u=edge[0],v=edge[1],wt=edge[2];
            if(d.find(u)!=d.find(v)){
                cnt++;
                cost+=wt;
                d.merge(u,v);
            }
            if(cnt==points.size()-1)
            break;
        }
        return cost;
    }
};
```

## 5 Swim in Rising Water

You are given an n x n integer matrix grid where each value grid[i][j] represents the elevation at that point (i, j).

It starts raining, and water gradually rises over time. At time t, the water level is t, meaning any cell with elevation less than equal to t is submerged or reachable.

You can swim from a square to another 4-directionally adjacent square if and only if the elevation of both squares individually are at most t. You can swim infinite distances in zero time. Of course, you must stay within the boundaries of the grid during your swim.

Return the minimum time until you can reach the bottom right square (n - 1, n - 1) if you start at the top left square (0, 0).

```cpp
class Solution {
public:
    int swimInWater(vector<vector<int>>& grid) {
        vector<vector<int>>dist(grid.size(),vector<int>(grid[0].size(),INT_MAX));
        priority_queue<vector<int>,vector<vector<int>>,greater<>>pq;
        pq.push({0,0,grid[0][0]});
        dist[0][0]=grid[0][0];
        while(!pq.empty()){
            int i=pq.top()[0],j=pq.top()[1],d=pq.top()[2];
            pq.pop();
            if(i-1>=0 && dist[i-1][j]>max(d,grid[i-1][j]))
            {
                dist[i-1][j]=max(d,grid[i-1][j]);
                pq.push({i-1,j,dist[i-1][j]});
            }
            if(j-1>=0 && dist[i][j-1]>max(d,grid[i][j-1]))
            {
                dist[i][j-1]=max(d,grid[i][j-1]);
                pq.push({i,j-1,dist[i][j-1]});
            }
            if(i+1<grid.size() && dist[i+1][j]>max(d,grid[i+1][j]))
            {
                dist[i+1][j]=max(d,grid[i+1][j]);
                pq.push({i+1,j,dist[i+1][j]});
            }
            if(j+1<grid[0].size() && dist[i][j+1]>max(d,grid[i][j+1]))
            {
                dist[i][j+1]=max(d,grid[i][j+1]);
                pq.push({i,j+1,dist[i][j+1]});
            }
        }
        return dist[grid.size()-1][grid[0].size()-1];
    }
};
```
