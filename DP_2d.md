## 1 Unique Paths

There is a robot on an m x n grid. The robot is initially located at the top-left corner (i.e., grid[0][0]). The robot tries to move to the bottom-right corner (i.e., grid[m - 1][n - 1]). The robot can only move either down or right at any point in time.

Given the two integers m and n, return the number of possible unique paths that the robot can take to reach the bottom-right corner.

The test cases are generated so that the answer will be less than or equal to 2 * 109.

```cpp
class Solution {
public:
    int uniquePaths(int m, int n) {
        vector<vector<int>>dp(m,vector<int>(n,0));
        dp[0][0]=1;
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(i-1>=0)
                dp[i][j]+=dp[i-1][j];
                if(j-1>=0)
                dp[i][j]+=dp[i][j-1];
            }
        }
        return dp[m-1][n-1];
    }
};
```

## 2 Unique Paths II

You are given an m x n integer array grid. There is a robot initially located at the top-left corner (i.e., grid[0][0]). The robot tries to move to the bottom-right corner (i.e., grid[m - 1][n - 1]). The robot can only move either down or right at any point in time.

An obstacle and space are marked as 1 or 0 respectively in grid. A path that the robot takes cannot include any square that is an obstacle.

Return the number of possible unique paths that the robot can take to reach the bottom-right corner.

The testcases are generated so that the answer will be less than or equal to 2 * 109.

```cpp
class Solution {
public:
    int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
        if(obstacleGrid[obstacleGrid.size()-1][obstacleGrid[0].size()-1]==1)
        return 0;
        vector<vector<int>>dp(obstacleGrid.size(),vector<int>(obstacleGrid[0].size(),0));
        dp[0][0]=1;
        for(int i=0;i<obstacleGrid.size();i++){
            for(int j=0;j<obstacleGrid[0].size();j++){
                if(obstacleGrid[i][j]==1)
                continue;
                if(i-1>=0 && obstacleGrid[i-1][j]==0)
                dp[i][j]+=dp[i-1][j];
                if(j-1>=0 && obstacleGrid[i][j-1]==0)
                dp[i][j]+=dp[i][j-1];
            }
        }
        return dp[obstacleGrid.size()-1][obstacleGrid[0].size()-1];
    }
};
```
