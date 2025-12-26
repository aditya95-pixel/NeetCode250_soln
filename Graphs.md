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
