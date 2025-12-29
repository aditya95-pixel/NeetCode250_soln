## 1 Island Perimeter

You are given row x col grid representing a map where grid[i][j] = 1 represents land and grid[i][j] = 0 represents water.

Grid cells are connected horizontally/vertically (not diagonally). The grid is completely surrounded by water, and there is exactly one island (i.e., one or more connected land cells).

The island doesn't have "lakes", meaning the water inside isn't connected to the water around the island. One cell is a square with side length 1. The grid is rectangular, width and height don't exceed 100. Determine the perimeter of the island.

```cpp
class Solution {
public:
    int islandPerimeter(vector<vector<int>>& grid) {
        int res=0;
        for(int i=0;i<grid.size();i++){
            for(int j=0;j<grid[0].size();j++)
            {
                if(grid[i][j]==1)
                {
                    if((i-1>=0 && grid[i-1][j]==0) || i==0)
                    res++;
                    if((j-1>=0 && grid[i][j-1]==0) || j==0)
                    res++;
                    if((i+1<grid.size() && grid[i+1][j]==0) || i==grid.size()-1)
                    res++;
                    if((j+1<grid[0].size() && grid[i][j+1]==0) || j==grid[0].size()-1)
                    res++;
                }
            }
        }
        return res;
    }
};
```

## 2 Verifying an Alien Dictionary

In an alien language, surprisingly, they also use English lowercase letters, but possibly in a different order. The order of the alphabet is some permutation of lowercase letters.

Given a sequence of words written in the alien language, and the order of the alphabet, return true if and only if the given words are sorted lexicographically in this alien language.

```cpp
class Solution {
public:
    bool isAlienSorted(vector<string>& words, string order) {
        vector<int>mp(26);
        for(int i=0;i<26;i++)
        mp[order[i]-'a']=i;
        for(int i=0;i<words.size();i++){
            for(int j=i+1;j<words.size();j++){
                bool chk=0;
                for(int k=0;k<min(words[i].size(),words[j].size());k++){
                    if(words[i][k]==words[j][k])
                    continue;
                    else if(mp[words[i][k]-'a']<mp[words[j][k]-'a'])
                    {
                        chk=1;
                        break;
                    }
                    else if(mp[words[i][k]-'a']>mp[words[j][k]-'a'])
                    return 0;
                }
                if(!chk && words[i].size()>words[j].size())
                return 0;
            }
        }
        return 1;
    }
};
```

## 3 Find the Town Judge

In a town, there are n people labeled from 1 to n. There is a rumor that one of these people is secretly the town judge.

If the town judge exists, then:

The town judge trusts nobody.
Everybody (except for the town judge) trusts the town judge.
There is exactly one person that satisfies properties 1 and 2.
You are given an array trust where trust[i] = [ai, bi] representing that the person labeled ai trusts the person labeled bi. If a trust relationship does not exist in trust array, then such a trust relationship does not exist.

Return the label of the town judge if the town judge exists and can be identified, or return -1 otherwise.

```cpp
class Solution {
public:
    int findJudge(int n, vector<vector<int>>& trust) {
        vector<vector<int>>adj(n+1,vector<int>(n+1,0));
        for(auto &edge:trust)
            adj[edge[0]][edge[1]]=1;
        for(int i=1;i<=n;i++){
            bool chk=1;
            for(int j=1;j<=n;j++){
                if(adj[i][j]==1){
                    chk=0;
                    break;
                }
            }
            if(!chk)
            continue;
            int judge=i;
            for(int j=1;j<=n;j++){
                if(j==judge)
                continue;
                if(adj[j][i]==0)
                {
                    chk=0;
                    break;
                }
            }
            if(chk)
            return judge;
        }
        return -1;
    }
};
```

## 4 Number of Islands

Given an m x n 2D binary grid grid which represents a map of '1's (land) and '0's (water), return the number of islands.

An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

```cpp
class Solution {
public:
    int numIslands(vector<vector<char>>& grid) {
        int cnt=0;
        vector<vector<bool>>vis(grid.size(),vector<bool>(grid[0].size(),0));
        for(int i=0;i<grid.size();i++){
            for(int j=0;j<grid[0].size();j++){
                if(grid[i][j]=='1' && !vis[i][j]){
                    cnt++;
                    queue<pair<int,int>>q;
                    q.push({i,j});
                    vis[i][j]=1;
                    while(!q.empty()){
                        int i1=q.front().first,j1=q.front().second;
                        q.pop();
                        if(i1-1>=0 && grid[i1-1][j1]=='1' && !vis[i1-1][j1])
                        {
                            q.push({i1-1,j1});
                            vis[i1-1][j1]=1;
                        }
                        if(j1-1>=0 && grid[i1][j1-1]=='1' && !vis[i1][j1-1])
                        {
                            q.push({i1,j1-1});
                            vis[i1][j1-1]=1;
                        }
                        if(i1+1<grid.size() && grid[i1+1][j1]=='1' && !vis[i1+1][j1])
                        {
                            q.push({i1+1,j1});
                            vis[i1+1][j1]=1;
                        }
                        if(j1+1<grid[0].size() && grid[i1][j1+1]=='1' && !vis[i1][j1+1])
                        {
                            q.push({i1,j1+1});
                            vis[i1][j1+1]=1;
                        }
                    }
                }
            }
        }
        return cnt;
    }
};
```

## 5 Max Area of Island

You are given an m x n binary matrix grid. An island is a group of 1's (representing land) connected 4-directionally (horizontal or vertical.) You may assume all four edges of the grid are surrounded by water.

The area of an island is the number of cells with a value 1 in the island.

Return the maximum area of an island in grid. If there is no island, return 0.

```cpp
class Solution {
public:
    int maxAreaOfIsland(vector<vector<int>>& grid) {
        int maxcnt=0;
        vector<vector<bool>>vis(grid.size(),vector<bool>(grid[0].size(),0));
        for(int i=0;i<grid.size();i++){
            for(int j=0;j<grid[0].size();j++){
                if(grid[i][j]==1 && !vis[i][j]){
                    int cnt=0;
                    queue<pair<int,int>>q;
                    q.push({i,j});
                    vis[i][j]=1;
                    while(!q.empty()){
                        int i1=q.front().first,j1=q.front().second;
                        q.pop();
                        cnt++;
                        if(i1-1>=0 && grid[i1-1][j1]==1 && !vis[i1-1][j1])
                        {
                            q.push({i1-1,j1});
                            vis[i1-1][j1]=1;
                        }
                        if(j1-1>=0 && grid[i1][j1-1]==1 && !vis[i1][j1-1])
                        {
                            q.push({i1,j1-1});
                            vis[i1][j1-1]=1;
                        }
                        if(i1+1<grid.size() && grid[i1+1][j1]==1 && !vis[i1+1][j1])
                        {
                            q.push({i1+1,j1});
                            vis[i1+1][j1]=1;
                        }
                        if(j1+1<grid[0].size() && grid[i1][j1+1]==1 && !vis[i1][j1+1])
                        {
                            q.push({i1,j1+1});
                            vis[i1][j1+1]=1;
                        }
                    }
                    maxcnt=max(maxcnt,cnt);
                }
            }
        }
        return maxcnt;
    }
};
```

## 6 Clone Graph

Given a reference of a node in a connected undirected graph.

Return a deep copy (clone) of the graph.

Each node in the graph contains a value (int) and a list (List[Node]) of its neighbors.

class Node {
    public int val;
    public List<Node> neighbors;
}
 

Test case format:

For simplicity, each node's value is the same as the node's index (1-indexed). For example, the first node with val == 1, the second node with val == 2, and so on. The graph is represented in the test case using an adjacency list.

An adjacency list is a collection of unordered lists used to represent a finite graph. Each list describes the set of neighbors of a node in the graph.

The given node will always be the first node with val = 1. You must return the copy of the given node as a reference to the cloned graph.

```cpp
class Solution {
public:
    Node* cloneGraph(Node* node) {
        if(!node)
        return NULL;
        Node* newHead=new Node(node->val);
        queue<Node*>q1,q2;
        q1.push(node);
        q2.push(newHead);
        map<int,Node*>mp;
        mp[node->val]=newHead;
        while(!q1.empty()){
            Node*n1=q1.front(),*n2=q2.front();
            q1.pop();
            q2.pop();
            for(auto n:n1->neighbors){
                if(!mp.count(n->val))
                {
                    mp[n->val]=new Node(n->val);
                    q1.push(n);
                    q2.push(mp[n->val]);
                    n2->neighbors.push_back(mp[n->val]);
                }else
                n2->neighbors.push_back(mp[n->val]);
            }
        }
        return newHead;
    }
};
```

## 7 Islands and Treasure

You are given a m×n 2D grid initialized with these three possible values:

-1 - A water cell that can not be traversed.
0 - A treasure chest.
INF - A land cell that can be traversed. We use the integer 2^31 - 1 = 2147483647 to represent INF.
Fill each land cell with the distance to its nearest treasure chest. If a land cell cannot reach a treasure chest then the value should remain INF.

Assume the grid can only be traversed up, down, left, or right.

Modify the grid in-place.

```cpp
class Solution {
public:
    void islandsAndTreasure(vector<vector<int>>& grid) {
        const int INF=2147483647;
        queue<vector<int>>q;
        vector<vector<bool>>vis(grid.size(),vector<bool>(grid[0].size(),0));
        for(int i=0;i<grid.size();i++){
            for(int j=0;j<grid[0].size();j++){
                if(grid[i][j]==0){
                    q.push({i,j,0});
                    vis[i][j]=1;
                }
            }
        }
        while(!q.empty()){
            int i=q.front()[0],j=q.front()[1],dist=q.front()[2];
            q.pop();
            if(i-1>=0 && grid[i-1][j]==INF && !vis[i-1][j]){
                vis[i-1][j]=1;
                grid[i-1][j]=dist+1;
                q.push({i-1,j,dist+1});
            }
            if(j-1>=0 && grid[i][j-1]==INF && !vis[i][j-1]){
                vis[i][j-1]=1;
                grid[i][j-1]=dist+1;
                q.push({i,j-1,dist+1});
            }
            if(i+1<grid.size() && grid[i+1][j]==INF && !vis[i+1][j]){
                vis[i+1][j]=1;
                grid[i+1][j]=dist+1;
                q.push({i+1,j,dist+1});
            }
            if(j+1<grid[0].size() && grid[i][j+1]==INF && !vis[i][j+1]){
                vis[i][j+1]=1;
                grid[i][j+1]=dist+1;
                q.push({i,j+1,dist+1});
            }
        }
    }
};
```

## 8 Rotting Oranges

You are given an m x n grid where each cell can have one of three values:

0 representing an empty cell,
1 representing a fresh orange, or
2 representing a rotten orange.
Every minute, any fresh orange that is 4-directionally adjacent to a rotten orange becomes rotten.

Return the minimum number of minutes that must elapse until no cell has a fresh orange. If this is impossible, return -1.

```cpp
class Solution {
public:
    int orangesRotting(vector<vector<int>>& grid) {
        queue<vector<int>>q;
        for(int i=0;i<grid.size();i++){
            for(int j=0;j<grid[0].size();j++){
                if(grid[i][j]==2)
                    q.push({i,j,0});
            }
        }
        int time=0;
        while(!q.empty()){
            int i=q.front()[0],j=q.front()[1],t=q.front()[2];
            time=t;
            q.pop();
            if(i-1>=0 && grid[i-1][j]==1)
            {
                q.push({i-1,j,t+1});
                grid[i-1][j]=2;
            }
            if(j-1>=0 && grid[i][j-1]==1)
            {
                q.push({i,j-1,t+1});
                grid[i][j-1]=2;
            }
            if(i+1<grid.size() && grid[i+1][j]==1)
            {
                q.push({i+1,j,t+1});
                grid[i+1][j]=2;
            }
            if(j+1<grid[0].size() && grid[i][j+1]==1)
            {
                q.push({i,j+1,t+1});
                grid[i][j+1]=2;
            }
        }
        for(int i=0;i<grid.size();i++){
            for(int j=0;j<grid[0].size();j++){
                if(grid[i][j]==1)
                return -1;
            }
        }
        return time;
    }
};
```

## 9 Pacific Atlantic Water Flow

There is an m x n rectangular island that borders both the Pacific Ocean and Atlantic Ocean. The Pacific Ocean touches the island's left and top edges, and the Atlantic Ocean touches the island's right and bottom edges.

The island is partitioned into a grid of square cells. You are given an m x n integer matrix heights where heights[r][c] represents the height above sea level of the cell at coordinate (r, c).

The island receives a lot of rain, and the rain water can flow to neighboring cells directly north, south, east, and west if the neighboring cell's height is less than or equal to the current cell's height. Water can flow from any cell adjacent to an ocean into the ocean.

Return a 2D list of grid coordinates result where result[i] = [ri, ci] denotes that rain water can flow from cell (ri, ci) to both the Pacific and Atlantic oceans.

```cpp
class Solution {
public:
    vector<vector<bool>> processpac(vector<vector<int>>& heights){
        vector<vector<bool>>vis(heights.size(),vector<bool>(heights[0].size(),0));
        queue<vector<int>>q;
        q.push({0,0,heights[0][0]});
        vis[0][0]=1;
        for(int i=1;i<heights.size();i++)
        {
            q.push({i,0,heights[i][0]});
            vis[i][0]=1;
        }
        for(int j=1;j<heights[0].size();j++)
        {
            q.push({0,j,heights[0][j]});
            vis[0][j]=1;
        }
        while(!q.empty()){
            int i=q.front()[0],j=q.front()[1],h=q.front()[2];
            q.pop();
            if(i-1>=0 && !vis[i-1][j] && heights[i-1][j]>=heights[i][j])
            {
                q.push({i-1,j,heights[i-1][j]});
                vis[i-1][j]=1;
            }
            if(j-1>=0 && !vis[i][j-1] && heights[i][j-1]>=heights[i][j])
            {
                q.push({i,j-1,heights[i][j-1]});
                vis[i][j-1]=1;
            }
            if(i+1<heights.size() && !vis[i+1][j] && heights[i+1][j]>=heights[i][j])
            {
                q.push({i+1,j,heights[i+1][j]});
                vis[i+1][j]=1;
            }
            if(j+1<heights[0].size() && !vis[i][j+1] && heights[i][j+1]>=heights[i][j])
            {
                q.push({i,j+1,heights[i][j+1]});
                vis[i][j+1]=1;
            }
        }
        return vis;
    }
    vector<vector<bool>> processatl(vector<vector<int>>& heights){
        vector<vector<bool>>vis(heights.size(),vector<bool>(heights[0].size(),0));
        queue<vector<int>>q;
        q.push({(int)heights.size()-1,(int)heights[0].size()-1,heights[heights.size()-1][heights[0].size()-1]});
        vis[heights.size()-1][heights[0].size()-1]=1;
        for(int i=0;i<heights.size()-1;i++)
        {
            q.push({i,(int)heights[0].size()-1,heights[i][heights[0].size()-1]});
            vis[i][heights[0].size()-1]=1;
        }
        for(int j=0;j<heights[0].size()-1;j++)
        {
            q.push({(int)heights.size()-1,j,heights[heights.size()-1][j]});
            vis[heights.size()-1][j]=1;
        }
        while(!q.empty()){
            int i=q.front()[0],j=q.front()[1],h=q.front()[2];
            q.pop();
            if(i-1>=0 && !vis[i-1][j] && heights[i-1][j]>=heights[i][j])
            {
                q.push({i-1,j,heights[i-1][j]});
                vis[i-1][j]=1;
            }
            if(j-1>=0 && !vis[i][j-1] && heights[i][j-1]>=heights[i][j])
            {
                q.push({i,j-1,heights[i][j-1]});
                vis[i][j-1]=1;
            }
            if(i+1<heights.size() && !vis[i+1][j] && heights[i+1][j]>=heights[i][j])
            {
                q.push({i+1,j,heights[i+1][j]});
                vis[i+1][j]=1;
            }
            if(j+1<heights[0].size() && !vis[i][j+1] && heights[i][j+1]>=heights[i][j])
            {
                q.push({i,j+1,heights[i][j+1]});
                vis[i][j+1]=1;
            }
        }
        return vis;
    }
    vector<vector<int>> pacificAtlantic(vector<vector<int>>& heights) {
        vector<vector<bool>>pac=processpac(heights),atl=processatl(heights);
        vector<vector<int>>res;
        for(int i=0;i<heights.size();i++){
            for(int j=0;j<heights[0].size();j++){
                if(pac[i][j] && atl[i][j])
                res.push_back({i,j});
            }
        }
        return res;
    }
};
```

## 10 Surrounded Regions

You are given an m x n matrix board containing letters 'X' and 'O', capture regions that are surrounded:

Connect: A cell is connected to adjacent cells horizontally or vertically.
Region: To form a region connect every 'O' cell.
Surround: The region is surrounded with 'X' cells if you can connect the region with 'X' cells and none of the region cells are on the edge of the board.
To capture a surrounded region, replace all 'O's with 'X's in-place within the original board. You do not need to return anything.

```cpp
class Solution {
public:
    void solve(vector<vector<char>>& grid) {
        for(int i=0;i<grid.size();i++){
            for(int j=0;j<grid[0].size();j++){
                if(grid[i][j]=='O' && (i==0 || j==0 || i==grid.size()-1 || j==grid[0].size()-1))
                {
                    grid[i][j]='.';
                    queue<pair<int,int>>q;
                    q.push({i,j});
                    while(!q.empty()){
                        int i1=q.front().first,j1=q.front().second;
                        q.pop();
                        if(i1-1>=0 && grid[i1-1][j1]=='O')
                        {
                            grid[i1-1][j1]='.';
                            q.push({i1-1,j1});
                        }
                        if(j1-1>=0 && grid[i1][j1-1]=='O')
                        {
                            grid[i1][j1-1]='.';
                            q.push({i1,j1-1});
                        }
                        if(i1+1<grid.size() && grid[i1+1][j1]=='O')
                        {
                            grid[i1+1][j1]='.';
                            q.push({i1+1,j1});
                        }
                        if(j1+1<grid[0].size() && grid[i1][j1+1]=='O')
                        {
                            grid[i1][j1+1]='.';
                            q.push({i1,j1+1});
                        }
                    }
                }
            }
        }
        for(int i=0;i<grid.size();i++){
            for(int j=0;j<grid[0].size();j++){
                if(grid[i][j]=='O')
                grid[i][j]='X';
                else if(grid[i][j]=='.')
                grid[i][j]='O';
            }
        }
    }
};
```

## 11 Open the Lock

You have a lock in front of you with 4 circular wheels. Each wheel has 10 slots: '0', '1', '2', '3', '4', '5', '6', '7', '8', '9'. The wheels can rotate freely and wrap around: for example we can turn '9' to be '0', or '0' to be '9'. Each move consists of turning one wheel one slot.

The lock initially starts at '0000', a string representing the state of the 4 wheels.

You are given a list of deadends dead ends, meaning if the lock displays any of these codes, the wheels of the lock will stop turning and you will be unable to open it.

Given a target representing the value of the wheels that will unlock the lock, return the minimum total number of turns required to open the lock, or -1 if it is impossible.

```cpp
class Solution {
public:
    int openLock(vector<string>& deadends, string target) {
        set<string>seto(deadends.begin(),deadends.end());
        if(seto.count("0000"))
        return -1;
        queue<pair<string,int>>q;
        q.push({"0000",0});
        set<string>vis;
        vis.insert("0000");
        while(!q.empty()){
            string s=q.front().first;
            int cnt=q.front().second;
            q.pop();
            if(s==target)
            return cnt;
            for(int i=0;i<4;i++){
                //increase
                if(s[i]=='9')
                s[i]='0';
                else
                s[i]=(char)(s[i]+1);
                if(!seto.count(s) && !vis.count(s))
                {
                    q.push({s,cnt+1});
                    vis.insert(s);
                }
                if(s[i]=='0')
                s[i]='9';
                else
                s[i]=(char)(s[i]-1);
                //decrease
                if(s[i]=='0')
                s[i]='9';
                else
                s[i]=(char)(s[i]-1);
                if(!seto.count(s) && !vis.count(s))
                {
                    q.push({s,cnt+1});
                    vis.insert(s);
                }
                if(s[i]=='9')
                s[i]='0';
                else
                s[i]=(char)(s[i]+1);
            }
        }
        return -1;
    }
};
```

## 12 Course Schedule

There are a total of numCourses courses you have to take, labeled from 0 to numCourses - 1. You are given an array prerequisites where prerequisites[i] = [ai, bi] indicates that you must take course bi first if you want to take course ai.

For example, the pair [0, 1], indicates that to take course 0 you have to first take course 1.
Return true if you can finish all courses. Otherwise, return false.

```cpp
class Solution {
public:
    bool canFinish(int V, vector<vector<int>>& edges) {
        vector<int>indeg(V,0);
        vector<vector<int>>adj(V);
        for(auto &edge:edges)
        {
            adj[edge[1]].push_back(edge[0]);
            indeg[edge[0]]++;
        }
        queue<int>q;
        for(int i=0;i<V;i++){
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
                if(indeg[v]==0)
                q.push(v);
            }
        }
        return res.size()==V;
    }
};
```
