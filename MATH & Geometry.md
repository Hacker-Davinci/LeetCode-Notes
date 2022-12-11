就是觀察題 + 實作 的考驗

73.Set Matrix Zeroes

```CPP
class Solution {
public:
    void setZeroes(vector<vector<int>>& matrix) {
        int m = matrix.size(), n = matrix[0].size();
        bool row_zero = false, col_zero = false;
        // set the up and left border to zeros 
        for(int i = 0; i < m; ++i){
            for(int j = 0; j < n; ++j){
                if(matrix[i][j] == 0){
                    if(i == 0) row_zero = true;
                    if(j == 0) col_zero = true;
                    matrix[0][j] = 0;
                    matrix[i][0] = 0;
                }
            }
        }
        // set the  rows 
        for(int i = 1; i < m; ++i){
            if(matrix[i][0] == 0){
                for(int j = 1; j < n; ++j)
                    matrix[i][j] = 0;
            }
        }
        // set the cols 
        for(int j = 1; j < n; ++j){
            if(matrix[0][j] == 0){
                for(int i = 1; i < m; ++i)
                    matrix[i][j] = 0;
            }
        }
        // set the 0, 0 case 
        if(row_zero){
            for(int i = 0; i < n; ++i)
                matrix[0][i] = 0;
        }
        if(col_zero){
            for(int i = 0; i < m; ++i)
                matrix[i][0] = 0;
        }
    }
};
```


1260.Shift 2D Grid
```cpp
class Solution {
public:
    vector<vector<int>> shiftGrid(vector<vector<int>>& grid, int k) {
        int m = grid.size(), n = grid[0].size();
        int size = m * n;
        if(k == 0 || k == size) return grid;
        vector<vector<int>> new_grid = grid;
        for(int i = 0; i < m; ++i){
            for(int j = 0; j < n; ++j){
                int index = i * n + j + k;
                index %= size;
                new_grid[(index/n)%m][index%n] = grid[i][j];
            }
        }
        return new_grid;
    }
};
```



2013.Detect Squares
超級有趣的好題目 XD
GOOGLE 我來了
```cpp
class DetectSquares {
public:
    unordered_map<int, unordered_map<int, int>> mp;
    set<pair<int, int>> st;
    DetectSquares() {
        mp.clear();
        st.clear();
    }
    void add(vector<int> point) {
        int x = point[0], y = point[1];
        st.insert({x, y});
        mp[x][y]++;
    }
    int count(vector<int> point) {
        int cnt = 0;
        int x = point[0], y = point[1];
        for(auto &p: st){
            int dx = p.first, dy = p.second;
            if(dx == x || dy == y || (abs(x-dx) != abs(y-dy)) ) continue;
            cnt += (mp[x][dy] * mp[dx][y] * mp[dx][dy]);
        }
        return cnt;
    }   
};
```


1041.Robot Bounded In Circle
```cpp
今天德˙最後

```


6.Zigzag Conversion
看一下怎ㄇ做ㄉ



2028.Find Missing Observations
原來可以這樣分配
想到聯立方程式
會想到 exgcd，但 leetcode 不考 exgcd
```cpp
class Solution {
public:
    vector<int> missingRolls(vector<int>& rolls, int mean, int n) {
        int m = rolls.size();
        int sum = (n+m) * mean;
        int correct = 0;
        for(auto &u: rolls) 
            correct += u;
        int left = sum - correct;
        if(left > 6*n || left < n) return vector<int>();
        // 6 * k + 1 - (n-k) = left
        int base = left/n, remainder = left%n;
        vector<int> ans(n, base);
        for(int i = 0; i < remainder; ++i)
            ans[i]++;
        return ans;
    }
};
```


43.Multiply Strings
PASS
超麻煩

