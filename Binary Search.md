舊版本 Template
```cpp
>= for lowerbound
>  for upper bound
if r == nums.size() means that 
target is larger than every element in nums
l 放置 -1, r 放置 最大可能的答案

int bs(vector<int>& nums, int target){
        int l = -1, r = nums.size();
        int mid;
        while(l < r-1){
            mid = (l+r) / 2;
            if(nums[mid] >= target) r = mid;
            else l = mid;
        }
        return (r != nums.size() && nums[r] == target )? r : -1;
    }
```



新版本 Template
```cpp
[l, r)
int binary_search(vector<int> &nums, int target){
	int l = 0, r = nums.size(), mid;
	while(l < r){
		mid = (l) + (r-l) / 2;
		if(nums[mid] == target) return mid;  [l, m)
		if(nums[mid] > target) r = mid;      [m+1, r)
		else l = mid+1;
	}
	return -1;  // not found 
}
```
或是
```cpp
upper bound
[l, r)
int binary_search(vector<int> &nums, int target){
	int l = 0, r = nums.size(), mid;
	while(l < r){
		mid = (l) + (r-l) / 2;
		if(nums[mid] > target) r = mid;   [l, m)    
		else l = mid+1;                   [m+1, r)
	}
	return (l == nums.size()) ? not found: found;  // not found 
}
```
或是
```cpp
lower bound
[l, r)
int binary_search(vector<int> &nums, int target){
	int l = 0, r = nums.size(), mid;
	while(l < r){
		mid = (l) + (r-l) / 2;
		if(nums[mid] > target) r = mid;   [l, m)    
		else l = mid+1;                   [m+1, r)
	}
	return l;  // not found 
}
```


441.Arranging Coins
考前必複習

```cpp
拿到 小於等於 的 值 符合本題情況
class Solution {
public:
    int arrangeCoins(int n) {
        long long l = 0, r = n+1;
        long long mid;
        while(l < r-1){
            mid = (l+r) / 2;
            long long coins = (mid * (mid+1)) / 2;
            if(coins > n) r = mid;
            else l = mid;
        } 
        return r-1;
    }
};
```


977.Squares of a Sorted Array
```cpp
class Solution {
public:
    // merge sort ?
    vector<int> sortedSquares(vector<int>& nums) {
        vector<int> neg, pos;
        int k = nums.size();
        for(int i = 0; i < k; ++i){
            if(nums[i] < 0) neg.push_back(nums[i]*nums[i]);
            else pos.push_back(nums[i]*nums[i]);
        }
        reverse(neg.begin(), neg.end());
        int i = 0, j = 0;
        int n = neg.size(), m = pos.size();
        while(i < n && j < m){
            if(neg[i] < pos[j]) nums[i+j] = neg[i], i++;
            else nums[i+j] = pos[j], j++;
        }
        while(i < n) nums[i+j] = neg[i], i++;
        while(j < m) nums[i+j] = pos[j], j++;
        return nums;
    }
};
```
```cpp
class Solution {
public:
    vector<int> sortedSquares(vector<int>& A) {
        vector<int> res(A.size());
        int l = 0, r = A.size() - 1;
        for (int k = A.size() - 1; k >= 0; k--) {
            if (abs(A[r]) > abs(A[l])) res[k] = A[r] * A[r--];
            else res[k] = A[l] * A[l++];
        }
        return res;
    }
};
```

367.Valid Perfect Square
```cpp
class Solution {
public:
    bool isPerfectSquare(long long num) {
        long long l = 0, r = num+1;
        long long mid;
        while(l < r-1){
            mid = (l+r) / 2;
            if(mid*mid == num) return true;
            if(mid*mid > num) r = mid;
            else l = mid;
        }
        return false;
    }
};
```


74.Search a 2D Matrix
必須回來看 
原因是因為
就算你搜尋到了可能位置
答案還是有可能不再這 !

以及 對於 binary search 細節的掌握
邊界
l = -1, r = 最大可能的VALUE
mid = (l+r) / 2; 不會溢位，不會碰到l, r 這兩個邊界
但是 r 同時要放 可能的答案元素
```cpp
class Solution {
public:
    // binary search the row
    // binary search the column
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        int m = matrix.size(), n = matrix[0].size();
        int mid;
        int up, down, l, r;
        up = 0, down = m, l = 0, r = n;
        while(up < down){
            mid = up + (down-up) / 2;
            if(matrix[mid][n-1] >= target) down = mid;
            else up = mid+1;
        }
        if(up == m) return false;
        while(l < r){
            mid = l + (r-l) / 2;
            if(matrix[up][mid] >= target) r = mid;
            else l = mid+1;
        }
        if(l == n) return false;
        return matrix[up][l] == target;
    }
};
```


875.Koko Eating Bananas
一個很好的題目
藉由二分搜 去達成 n logn 的解法
二分搜 本身給你 logn 
每次 確認 n 
```cpp
binary search the number k
how to check it ?
class Solution {
public:
    int minEatingSpeed(vector<int>& piles, int h) {
        int l = 0, r = 1e9;
        int mid;
        while(l < r-1){
            mid = (l+r) / 2;
            if(isable(piles, h, mid)) r = mid;
            else l = mid;
        }
        return r;
    }
    inline bool isable(vector<int>& piles, int &h, int k){
        long long cur_h = 0;
        for(auto &u: piles){
            if(u%k == 0) cur_h += u/k;
            else cur_h += (u/k + 1);
        }
        return cur_h <= h;
    }
};
```
```cpp
class Solution {
public:
    int minEatingSpeed(vector<int>& piles, int h) {
        int l = 1, r = 1e9, mid;
        long long cur_h;
        while(l < r){
            mid = l + (r-l) / 2;
            cur_h = 0;
            for(auto &u: piles){
                if(u%mid == 0) cur_h += (u/mid);
                else cur_h += (u/mid + 1);
            }
            if(cur_h <= h) r = mid;
            else l = mid+1;
        }
        return l;
    }
};
```



33.Search in rotated sorted array
不知道怎麼做的時候
可以先把圖片 視覺化 畫出來
他能夠幫你釐清思緒
目前要先背起來了
![[Pasted image 20221210104636.png | 300]]
```cpp
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int l = -1, r = nums.size(), mid;
        while(l < r-1){
            mid = (l+r) / 2;
            if(nums[mid] == target) return mid;
            // left sorted array
            if(nums[mid] >= nums[l+1]){
                if(nums[mid] > target && nums[l+1] <= target)
                    r = mid;
                else 
                    l = mid;
            }else{
            // right sorted array
                if(nums[mid] < target && nums[r-1] >= target)
                    l = mid;
                else
                    r = mid;
            }            
        }
        return -1;
    }
};
```
```cpp
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int l = 0, r = nums.size(), mid;
        while(l < r){
            mid = l + (r-l) / 2;
            if(nums[mid] == target) return mid;
            // left sorted array
            if(nums[mid] >= nums[l]){
                if(target >= nums[l] && nums[mid] > target)
                    r = mid;
                else
                    l = mid+1;
            }else{
                if(target <= nums[r-1] && nums[mid] < target)
                    l = mid+1;
                else
                    r = mid;
            }
        }
        return -1;
    }
};
```


153.Find Minimum in Rotated Sorted Array
```cpp

```




162.Find Peak Element
```cpp

```



275.H-Index II
```cpp

```

