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
