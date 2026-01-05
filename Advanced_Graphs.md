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

## 6 Alien Dictionary

There is a foreign language which uses the latin alphabet, but the order among letters is not "a", "b", "c" ... "z" as in English.

You receive a list of non-empty strings words from the dictionary, where the words are sorted lexicographically based on the rules of this new language.

Derive the order of letters in this language. If the order is invalid, return an empty string. If there are multiple valid order of letters, return any of them.

A string a is lexicographically smaller than a string b if either of the following is true:

The first letter where they differ is smaller in a than in b.
a is a prefix of b and a.length < b.length.

```cpp
class Solution {
public:
    string foreignDictionary(vector<string>& words) {
        vector<int>exist(26,0),indeg(26,0);
        for(auto &word:words){
            for(auto &c:word)
            exist[c-'a']++;
        }
        vector<vector<int>>adj(26);
        for(int i=0;i<words.size()-1;i++){
            string&a=words[i],&b=words[i+1];
            int idx=0;
            while(idx<a.size() && idx<b.size() && a[idx]==b[idx])
            idx++;
            if(idx==b.size() && idx<a.size())
            return "";
            if(idx<a.size() && idx<b.size())
            {
                adj[a[idx]-'a'].push_back(b[idx]-'a');
                indeg[b[idx]-'a']++;
            }
        }
        queue<int>q;
        for(int i=0;i<26;i++){
            if(exist[i] && indeg[i]==0)
            q.push(i);
        }
        string res;
        while(!q.empty()){
            int u=q.front();
            q.pop();
            res+=(char)(u+'a');
            for(auto &v:adj[u]){
                indeg[v]--;
                if(indeg[v]==0)
                q.push(v);
            }
        }
        for(int i=0;i<26;i++){
            if(exist[i] && indeg[i]>0)
            return "";
        }
        return res;
    }
};
```

## 7 Cheapest Flights Within K Stops

There are n cities connected by some number of flights. You are given an array flights where flights[i] = [fromi, toi, pricei] indicates that there is a flight from city fromi to city toi with cost pricei.

You are also given three integers src, dst, and k, return the cheapest price from src to dst with at most k stops. If there is no such route, return -1.

```cpp
class Solution {
public:
    int findCheapestPrice(int n, vector<vector<int>>& flights, int src, int dst, int k) {
        vector<vector<pair<int,int>>>adj(n);
        for(auto &flight:flights){
            int &u=flight[0],&v=flight[1],&d=flight[2];
            adj[u].push_back({v,d});
        }
        queue<pair<int,int>>q;
        q.push({src,0});
        int stops=0;
        vector<int>dist(n,INT_MAX);
        dist[src]=0;
        while(!q.empty() && stops<=k){
            int sz=q.size();
            while(sz--){
                int u=q.front().first,cost=q.front().second;
                q.pop();
                for(auto &item:adj[u]){
                    int &v=item.first,&d=item.second;
                    if(dist[v]>cost+d)
                    {
                        dist[v]=cost+d;
                        q.push({v,dist[v]});
                    }
                }
            }
            stops++;
        }
        return (dist[dst]==INT_MAX)?-1:dist[dst];
    }
};
```

## 8 Find Critical and Pseudo-Critical Edges in Minimum Spanning Tree

Given a weighted undirected connected graph with n vertices numbered from 0 to n - 1, and an array edges where edges[i] = [ai, bi, weighti] represents a bidirectional and weighted edge between nodes ai and bi. A minimum spanning tree (MST) is a subset of the graph's edges that connects all vertices without cycles and with the minimum possible total edge weight.

Find all the critical and pseudo-critical edges in the given graph's minimum spanning tree (MST). An MST edge whose deletion from the graph would cause the MST weight to increase is called a critical edge. On the other hand, a pseudo-critical edge is that which can appear in some MSTs but not all.

Note that you can return the indices of the edges in any order.

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
        else
        return parent[u]=find(parent[u]);
    }
    void merge(int u,int v){
        u=find(u);
        v=find(v);
        if(rank[v]>rank[u])
        parent[u]=v;
        else if(rank[v]<rank[u])
        parent[v]=u;
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
    vector<vector<int>> findCriticalAndPseudoCriticalEdges(int n, vector<vector<int>>& edges) {
        vector<vector<int>>es;
        vector<vector<int>>res(2);
        for(int i=0;i<edges.size();i++){
            vector<int>&e=edges[i];
            es.push_back({e[0],e[1],e[2],i});
        }
        sort(es.begin(),es.end(),cmp);
        int mstwt=0,cnt=0;
        DSU d(n);
        for(auto &e:es){
            int u=e[0],v=e[1],wt=e[2];
            if(d.find(u)!=d.find(v)){
                d.merge(u,v);
                mstwt+=wt;
                cnt++;
            }
            if(cnt==n-1)
            break;
        }
        for(int i=0;i<es.size();i++){
            DSU d1(n);
            int wt1=0;
            cnt=0;
            for(int j=0;j<es.size();j++){
                if(i==j)
                continue;
                vector<int>&e=es[j];
                int u=e[0],v=e[1],wt=e[2];
                if(d1.find(u)!=d1.find(v)){
                    d1.merge(u,v);
                    wt1+=wt;
                    cnt++;
                }
                if(cnt==n-1)
                break;
            }
            if(mstwt!=wt1)
            {
                res[0].push_back(es[i][3]);
                continue;
            }
            DSU d2(n);
            d2.merge(es[i][0],es[i][1]);
            wt1=es[i][2];
            cnt=0;
            for(int j=0;j<es.size();j++){
                if(i==j)
                continue;
                vector<int>&e=es[j];
                int u=e[0],v=e[1],wt=e[2];
                if(d2.find(u)!=d2.find(v)){
                    d2.merge(u,v);
                    wt1+=wt;
                    cnt++;
                }
                if(cnt==n-1)
                break;
            }
            if(mstwt==wt1)
            res[1].push_back(es[i][3]);
        }
        return res;
    }
};
```

## 9 Build a Matrix With Conditions

You are given a positive integer k. You are also given:

a 2D integer array rowConditions of size n where rowConditions[i] = [abovei, belowi], and
a 2D integer array colConditions of size m where colConditions[i] = [lefti, righti].
The two arrays contain integers from 1 to k.

You have to build a k x k matrix that contains each of the numbers from 1 to k exactly once. The remaining cells should have the value 0.

The matrix should also satisfy the following conditions:

The number abovei should appear in a row that is strictly above the row at which the number belowi appears for all i from 0 to n - 1.
The number lefti should appear in a column that is strictly left of the column at which the number righti appears for all i from 0 to m - 1.
Return any matrix that satisfies the conditions. If no answer exists, return an empty matrix.

```cpp
class Solution {
public:
    vector<int>topoSort(int n,vector<vector<int>>&edges){
        vector<vector<int>>adj(n+1);
        vector<int>indeg(n+1,0);
        for(auto &e:edges)
        {
            adj[e[0]].push_back(e[1]);
            indeg[e[1]]++;
        }
        queue<int>q;
        for(int i=1;i<=n;i++){
            if(indeg[i]==0)
            q.push(i);
        }
        vector<int>res;
        while(!q.empty()){
            int u=q.front();
            q.pop();
            res.push_back(u);
            for(auto &v:adj[u]){
                indeg[v]--;
                if(!indeg[v])
                q.push(v);
            }
        }
        return res;
    }
    vector<vector<int>> buildMatrix(int k, vector<vector<int>>& rowConditions, vector<vector<int>>& colConditions) {
        vector<vector<int>>res;
        vector<int>colOrder=topoSort(k,colConditions);
        if(colOrder.size()!=k)
        return res;
        vector<int>rowOrder=topoSort(k,rowConditions);
        if(rowOrder.size()!=k)
        return res;
        vector<int>colIndex(k+1);
        for(int i=0;i<colOrder.size();i++)
        colIndex[colOrder[i]]=i;
        res.resize(k,vector<int>(k,0));
        for(int i=0;i<k;i++){
            int ele=rowOrder[i];
            res[i][colIndex[ele]]=ele;
        }
        return res;
    }
};
```

## 10 Greatest Common Divisor Traversal

You are given a 0-indexed integer array nums, and you are allowed to traverse between its indices. You can traverse between index i and index j, i != j, if and only if gcd(nums[i], nums[j]) > 1, where gcd is the greatest common divisor.

Your task is to determine if for every pair of indices i and j in nums, where i < j, there exists a sequence of traversals that can take us from i to j.

Return true if it is possible to traverse between all such pairs of indices, or false otherwise.

```cpp
class DSU{
    vector<int>parent,rank;
    public:
    DSU(int n){
        parent.resize(n);
        rank.resize(n,-1);
        iota(parent.begin(),parent.end(),0);
    }
    int find(int u){
        if(parent[u]==u)
        return u;
        else
        return parent[u]=find(parent[u]);
    }
    void merge(int u,int v){
        u=find(u);
        v=find(v);
        if(rank[u]>rank[v])
            parent[v]=u;
        else if(rank[v]>rank[u])
            parent[u]=v;
        else
        {
            parent[v]=u;
            rank[u]++;
        }
    }
};
class Solution {
public:
    bool canTraverseAllPairs(vector<int>& nums) {
        int maxo=*max_element(nums.begin(),nums.end());
        vector<int>sieve(maxo+1,0);
        sieve[1]=1;
        for(int i=2;i<=maxo;i++){
            if(sieve[i]!=0)
            continue;
            for(long long num=1LL*i*i;num<=maxo;num+=i)
            sieve[num]=i;
        }
        DSU d(nums.size()+maxo+1);
        for(int i=0;i<nums.size();i++){
            int num=nums[i];
            if(!sieve[num])
            {
                d.merge(i,nums.size()+num);
                continue;
            }
            while(num>1){
                int p=(!sieve[num])?num:sieve[num];
                d.merge(i,nums.size()+p);
                while(num%p==0)
                num/=p;
            }
        }
        int par=d.find(0);
        for(int i=1;i<nums.size();i++){
            if(d.find(i)!=par)
            return 0;
        }
        return 1;
    }
};
```
