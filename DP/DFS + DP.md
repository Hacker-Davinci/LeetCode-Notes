437.Path Sum III
```CPP
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

329.Longest Increasing Path in a Matrix (寫過)
```cpp
```

