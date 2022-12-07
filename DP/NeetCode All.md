
1D DP

Partition to K Equal Sum Subsets

Decode Ways

Word Break

Traingle
![[Pasted image 20221204130151.png|450 ]]
```cpp
dp[level][ith node] = triangle[level][ith] + min(dp[level+1][ith], dp[level+1][ith]);
class Solution {
public:
    int minimumTotal(vector<vector<int>>& triangle) {
        int n = triangle.size();
        vector<vector<int>> dp(n, vector<int>(n, -1));
        return solve(dp, triangle, 0, 0);
    }
    int solve(vector<vector<int>> &dp, vector<vector<int>>& triangle, int level, int i){
        if(level == triangle.size()) return 0;
        if(dp[level][i] != -1) return dp[level][i];
        int left = solve(dp, triangle, level+1, i);
        int right = solve(dp, triangle, level+1, i+1);
        return dp[level][i] = triangle[level][i] + min(left, right);
    }
};
```

Delete  and Earn

343.Integer Break
```cpp
class Solution {
public:
    dp[n] = max({dp[n-i] * i, (n-i) * i})
    if i == 2 : return 1;
    int integerBreak(int n) {
        vector<int> dp(n+1, -1);
        return solve(dp, n);
    }
    int solve(vector<int> &dp, int n){
        if(n <= 1) return 0;
        if(n == 2) return 1;
        if(dp[n] != -1) return dp[n];
        int ans = 0;
        for(int i = 1; i < n; ++i){
            ans = max({ans, i * solve(dp, n-i), i * (n-i)});
        }
        return dp[n] = ans;
    }
};
```
673.Number of Longest Increasing Subsequence

279.Perfect Squares
```cpp
class Solution {
public:
    the range of n ?
    will n > 0 ?
    will the range be onlt integer ??
    n will be from 1^2 to 100^2
    大膽的猜 很重要
    9
    4 + 4 + 1
	9
    27
    9 + 9 + 9
    16 + 9 + 1 + 1
    dp[i] = min(dp[i-squares[j]] for 1 <= j <= 100)  + 1
    dp[i] = 0 if i == 0
    O(N * 100)
    int numSquares(int n) {
        vector<int> dp(n+1, -1);
        vector<int> square(100+1, -1);
        for(int i = 1; i <= 100; ++i)
            square[i] = i * i;
        return solve(dp, square, n);
    }
    int solve(vector<int> &dp, vector<int> &square, int n){
        if(n < 0) return 2e9; // imposiible
        if(n == 0) return 0;
        if(dp[n] != -1) return dp[n];
        int ans = INT_MAX;
        for(int i = 1; i <= 100; ++i)
            ans = min(ans, solve(dp, square, n-square[i]));
        return dp[n] = 1 + ans;
    }

};
```
377.Combination Sum IV
```cpp
class Solution {
public:
    // the ragne of the target 
    // dp[cur_sum] = dp[cur_sum+nums[i]]; 
    // n = element size
    // k*n^2
    // will all element in nums > 0 ?
    // does it include permutation or just combination ?
    int combinationSum4(vector<int>& nums, int target) {
        int n = nums.size();
        vector<int> dp(target+1, -1);
        int cur_sum = 0;
        return solve(dp, nums, cur_sum, target);
    }
    int solve(vector<int> &dp, vector<int>& nums, int cur_sum, int target){
        if(cur_sum > target) return 0;
        if(cur_sum == target) return 1;
        if(dp[cur_sum] != -1) return dp[cur_sum];
        int n = nums.size();
        int cnt = 0;
        for(int i = 0; i < n; ++i)
            cnt += solve(dp, nums, cur_sum+nums[i], target);
        return dp[cur_sum] = cnt;
    }
};
```
256.Paint House II
```cpp
class Solution {
public:
    the range of n, k ?
    the range of the answer to long long int or int ?
    is it 100% have answer ?
    int minCostII(vector<vector<int>>& costs) {
        int n = costs.size();
        int k = costs[0].size();
        vector<vector<int>> dp(n, vector<int>(k, -1));
        int ans = INT_MAX;
        int house = 0;
        for(int i = 0; i < k; ++i)
            ans = min(ans, solve(dp, costs, house, i, k));
        return ans;
    }
    // N * K subproblems
    // the iteration costs k times
    int solve(vector<vector<int>> &dp, vector<vector<int>> &costs,
               int house, int color, int k){
        if(house == costs.size()) return 0;
        if(dp[house][color] != -1) return dp[house][color];
        int ans = INT_MAX;
        for(int i = 0; i < k; ++i){
            if(i == color) continue;
            ans = min(ans, solve(dp, costs, house+1, i, k));
        }
        return dp[house][color] = costs[house][color] + ans;
    }
};
```

1856.Maximum Subarray Min-Product

221.Maximal Square
![[Pasted image 20221204151954.png|200]]
```cpp
class Solution {
public:
	dp[i][j] = 1 + min(dp[i-1][j], dp[i][j-1], dp[i-1][j-1]) if matrix[i][j] == 1
    dp[i][j] = 0 if matrix[i][j] == 0
    if i < 0 || j < 0 return 0
    int maximalSquare(vector<vector<char>>& matrix) {
        int m = matrix.size(), n = matrix[0].size();
        vector<vector<int>> dp(m, vector<int>(n, -1));
        int ans = 0;
        for(int i = 0; i < m; ++i){
            for(int j = 0; j < n; ++j)
                ans = max(ans, solve(dp, matrix, i, j));
        }
        return ans*ans;
    }
    int solve(vector<vector<int>> &dp, vector<vector<char>>& matrix, int i, int j){
        if(i < 0 || j < 0) return 0;
        if(dp[i][j] != -1) return dp[i][j];
        return dp[i][j] = (matrix[i][j] == '0') ? 0 : \
                        1 + min({solve(dp, matrix, i-1, j),
                            solve(dp, matrix, i, j-1),
                            solve(dp, matrix, i-1, j-1)});
    }
};
```


64.Minimum Path Sum
```cpp
水題
class Solution {
public:
    int minPathSum(vector<vector<int>>& grid) {
        int m = grid.size(), n = grid[0].size();
        vector<vector<int>> dp(m, vector<int>(n, -1));
        return solve(dp, grid, m-1, n-1);       
    }
    int solve(vector<vector<int>> &dp, vector<vector<int>> &grid, int x, int y){
        if(x < 0 || y < 0) return 2e9;
        if(x == 0 && y == 0) return grid[0][0];
        if(dp[x][y] != -1) return dp[x][y];
        return dp[x][y] = grid[x][y] + min(solve(dp, grid, x-1, y), solve(dp, grid, x, y-1));
    }
};
```


877.Stone Game

97.Interleaving String

276.Paint Fence
![[Pasted image 20221207182323.png]]
```cpp
我的版本
class Solution {
public:
    // dp[pos][consecutive number]
    int dp[100005][3];
    int numWays(int n, int k) {
        memset(dp, 0, sizeof(dp));
        int ans = 0;
        int pos = 0;
        int consecutive_num = 0;
        // for colors
        ans += k * solve(pos+1, n, 0, k, 1);
        return ans;
    }
    inline int solve(int pos, int &n, int color, int &k, int consecutive_num){
        if(consecutive_num >= 3) return 0;
        if(pos == n) return 1;
        if(dp[pos][consecutive_num] != 0) return dp[pos][consecutive_num];
        int ans = 0;
        // the same color 
        ans += solve(pos+1, n, color, k, consecutive_num+1);
        // different colors
        
        ans += ((k-1) * solve(pos+1, n, (color+1)%2, k, 1));
        return dp[pos][consecutive_num] = ans;
    }
};
```
931.Minimum Falling Path Sum
判斷方式 很好想 難度不高
```cpp
dp[level][pos] = min({dp[level+1][pos], 
					dp[level+1][pos-1], 
					dp[level+1][pos+1]});
class Solution {
public:
    // answer range?
    // n range?
    int minFallingPathSum(vector<vector<int>>& matrix) {
        int n = matrix.size();
        vector<vector<int>> dp(n, vector<int>(n, -1));
        int ans = INT_MAX;
        for(int i = 0; i < n; ++i)
            ans = min(ans, solve(dp, matrix, 0, n, i));
        return ans;
    }
    int solve(vector<vector<int>> &dp, vector<vector<int>> &matrix,
                int level, int n, int pos){
        if(level == n) return 0;
        if(pos >= n || pos < 0) return INT_MAX;
        if(dp[level][pos] != -1) return dp[level][pos];
        int ans = INT_MAX;
        ans = min({solve(dp, matrix, level+1, n, pos+1),
                   solve(dp, matrix, level+1, n, pos),
                   solve(dp, matrix, level+1, n, pos-1) });
        return dp[level][pos] = ans + matrix[level][pos];
    }
};
```

