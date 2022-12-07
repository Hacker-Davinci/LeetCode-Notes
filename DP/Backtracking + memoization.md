
Regex/Wildcard, Partition to K Equal Sum Subsets


1547.Minimum Cost to Cut a Stick
```cpp
class Solution {
public:
    // dp[l][r] = 
    int minCost(int n, vector<int>& cuts) {
        cuts.push_back(0);
        cuts.push_back(n);
        sort(cuts.begin(), cuts.end());
        int m = cuts.size();
        vector<vector<long long >> dp(m, vector<long long>(m, INT_MAX));
        long long ans = 2e9;
        for(int i = 1; i < m-1; ++i)
            ans = min(ans, n + solve(dp, cuts, 0, i) + solve(dp, cuts, i, m-1));
        return ans;
    }
    long long solve(vector<vector<long long>> &dp, vector<int> &cuts, int l, int r){
        if((r-l) <= 1) return 0;
        
        if(dp[l][r] != INT_MAX) return dp[l][r];
        long long len = cuts[r] - cuts[l];
        
        for(int i = l+1; i < r; ++i)
            dp[l][r]  = min(dp[l][r], 
                        len + solve(dp, cuts, l, i) + solve(dp, cuts, i, r));
        
        return dp[l][r];
    }
};
```

matrix multiplication

