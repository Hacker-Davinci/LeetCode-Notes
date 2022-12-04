437.Path Sum III
**Input:** root = [10,5,-3,3,2,null,11,3,-2,null,1], targetSum = 8
**Output:** 3
**Explanation:** The paths that sum to 8 are shown.

![[Pasted image 20221204120444.png|300]]

```cpp
O(V^2)
dfs bruteforce 每個點都選擇作為起點，並一直做下去找出可能的答案
class Solution {
public:
    int pathSum(TreeNode* root, int target) {
        if(!root) return 0;
        return dfs(root, target) + pathSum(root->left, target) + pathSum(root->right, target);
    }
    int dfs(TreeNode* root, long long target){
        if(!root) return 0;
        return (target-root->val == 0) + dfs(root->left, target-root->val) + \
                            dfs(root->right, target-root->val);
    }
};
```

```cpp
O(V) 
prefix sum 搭配root 算下來 的結果 可以少precomupte 原本 brute force dfs 的 選或不選 每個節點走下去
ans += mp[query] 代表，前面有幾種可能到此root 'prefix sum' 為 'query' 拔掉該段路徑，此路徑就為 0
class Solution {
public:
    int pathSum(TreeNode* root, int target) {
        if(!root) return 0;
        unordered_map<long long, int> mp;
        long long cur = 0;
        int ans = 0;
        mp[0] = 1; // for prefix empty element
        dfs(mp, root, cur, target, ans);
        return ans;
    }
    void dfs(unordered_map<long long, int> &mp, TreeNode* root,
                long long cur, int target, int &ans){
        if(!root) return;
        cur += root->val;
        long long query = cur - target;
        ans += mp[query]; // prefix sum
        mp[cur]++;
        dfs(mp, root->left, cur, target, ans);
        dfs(mp, root->right, cur, target, ans);
        mp[cur]--;
        return;
    }
};
```
329.Longest Increasing Path in a Matrix 
利用 DP 存取 DFS 會 PRECOMPUTE 的地方
```cpp
class Solution {
public:
    int longestIncreasingPath(vector<vector<int>>& matrix) {
        int m=matrix.size(),n=matrix[0].size();
        vector<vector<int>> dp(m, vector<int>(n,-1));
        int ans = 0;
        for(int i = 0; i < m; ++i)
            for(int j = 0; j < n; ++j)
                if(dp[i][j] == -1)
                    ans = max(ans, dfs(i, j, m, n, dp, matrix));
        return ans;
    }
    int dfs(int r, int c, int m, int n, vector<vector<int>>&dp, vector<vector<int>>& matrix){
        if(dp[r][c] != -1) return dp[r][c];
        static int dirs[4][2] = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}};
        int step = 1;
        for(int i = 0; i < 4; ++i){
            int x = r + dirs[i][0], y = c + dirs[i][1];
            if(x >= 0 && x < m && y >= 0 && y < n && matrix[x][y] > matrix[r][c])
                step = max(step, 1 + dfs(x, y, m, n, dp, matrix));
        }
        return dp[r][c] = step;
    }
}; 
```


2328.Number of Increasing Paths in a Grid
```cpp
class Solution {
public:
    const int mod=1e9+7;
    int countPaths(vector<vector<int>>& grid) {
        int m=grid.size(),n=grid[0].size();
        vector<vector<int>> dp(m, vector<int>(n, -1));
        int ans = 0;
        for(int i = 0; i < m; ++i){
            for(int j = 0; j < n; ++j){
                ans = (ans + dfs(i, j, m, n, grid, dp)) % mod;
            }
        }
        return ans;
    }
    int dfs(int r, int c, int m, int n, vector<vector<int>>& grid, vector<vector<int>> &dp){
        if(dp[r][c]!= -1) return dp[r][c];
        int step = 1;
        static int dirs[4][2] = {{0, 1}, {1, 0}, {0, -1}, {-1, 0}};
        for(int i = 0; i < 4; ++i){
            int x = r+dirs[i][0], y = c+dirs[i][1];
            if(x >= 0 && x < m && y >= 0 && y < n && grid[x][y] > grid[r][c]){
                step = (step + dfs(x, y, m, n, grid, dp)) % mod;
            }
        }
        return dp[r][c] = step;
    }
};
```