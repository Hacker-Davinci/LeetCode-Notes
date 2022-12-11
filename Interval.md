
```cpp
vector<int> 刪除 插入 元素 練習
刪除第 0 個 元素
vec.erase(vec.begin())
刪除第 0 + index 個 元素
vec.erase(vec.begin() + index);
可以藉由 lower_bound - v.begin() 或 upper_bound - v.begin()
來獲得 pos 
vector.insert(pos, num)
vector.erase(pos)

v = {1, 2, 3}; 
1 2 3
v.insert(v.begin(), 4);
4 1 2 3
v.insert(v.begin()+1, 5);
4 5 1 2 3
v.insert(v.begin()+2, 6);
4 5 6 1 2 3
v.insert(v.end()-1, 10);
4 5 6 1 2 10 3
v.erase(v.begin()+2);
4 5 1 2 10 3
v.erase(v.begin()+3);
4 5 1 10 3
v.erase(v.end()-1);
4 5 1 10
v.end() is empty, not a iterator to integer !
```


```cpp
map 也可以當作 答案
mp.size() 
mp.erase(key) -> mp size 會減少 1
set 也可以erase key
st.erase(key) -> st.size 會減少 1
```


Insert Interval
```cpp
第一次做 其實不好想
最關鍵的是以下式子
newInterval = {min(newInterval[0], intervals[i][0]), 
				                max(newInterval[1], intervals[i][1])};
class Solution {
public:
    vector<vector<int>> insert(vector<vector<int>>& intervals, vector<int>& newInterval) {
        vector<vector<int>> res;
        int n = intervals.size();
        for(int i = 0; i < n; ++i){
            // case 1, don't need to merge and insert it before
            if(newInterval[1] < intervals[i][0]){
                res.push_back(newInterval);
                for(int j = i; j < n; ++j)
                    res.push_back(intervals[j]);
                return res;
            }
            // case 2, newinterval is earlier
            else if(newInterval[0] > intervals[i][1]){
                res.push_back(intervals[i]);
            }
            // case 3, merge the interval
            else{
                newInterval = {min(newInterval[0], intervals[i][0]), 
				                max(newInterval[1], intervals[i][1])};
            }
        }
        res.push_back(newInterval);
        return res;
    }
};

```



56.Merge Intervals
```cpp
class Solution {
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        sort(intervals.begin(), intervals.end());
        vector<vector<int>> res;
        vector<int> new_interval;
        int n = intervals.size();
        for(int i = 0; i < n; ++i){
            if(new_interval.size() == 0){
                new_interval = intervals[i];
            }else if(new_interval[1] < intervals[i][0]){
                res.push_back(new_interval);
                new_interval = intervals[i];
            }else{
                new_interval = {min(new_interval[0], intervals[i][0]),
				                 max(new_interval[1], intervals[i][1])};
            }
        }
        res.push_back(new_interval);
        return res;
    }
};
```


435.Non-overlapping Intervals
```cpp
要注意持續 更新 右邊邊界 才是對的
像是 
{1, 5}, {2, 3}, {3, 4}
其實就是要取 2 - 4
class Solution {
public:
    int eraseOverlapIntervals(vector<vector<int>>& intervals) {
        sort(intervals.begin(), intervals.end());
        int n = intervals.size();
        int cnt = 0;
        int right_boundary = intervals[0][1];
        for(int i = 1; i < n; ++i){
            if(right_boundary > intervals[i][0]){
                cnt++;
                right_boundary = min(right_boundary, intervals[i][1]);
            }
            else right_boundary = intervals[i][1];
        }
        return cnt;
    }
};

```

1288.Remove Covered Intervals
```cpp
觀察題
class Solution {
static bool cmp(vector<int> &a, vector<int> &b){
    if(a[0] == b[0]) return a[1] > b[1];
    return a[0] < b[0];
}
public:
    int removeCoveredIntervals(vector<vector<int>>& intervals) {
        sort(intervals.begin(), intervals.end(), cmp);
        // two pointer ???
        int cnt = 0;
        int n = intervals.size();
        for(int i = 1; i < n; ++i){
            if(intervals[i-1][1] >= intervals[i][1] && intervals[i][0] >= intervals[i-1][0]){
                swap(intervals[i], intervals[i-1]);
                // intervals[i] = intervals[i-1];
                cnt++;
            }
        }
        return n-cnt;
    }
};
```


436.Find Right Interval
```cpp
lower_bound
class Solution {
public:
    vector<int> findRightInterval(vector<vector<int>>& intervals) {
        map<int, int> record;
        vector<int> ans;
        int n = intervals.size();
        for(int i = 0; i < n; ++i){
            int start = intervals[i][0];
            record[start] = i;
        }
        for(int i = 0; i < n; ++i){
            int end = intervals[i][1];
            auto it = record.lower_bound(end);
            if(it == record.end()) ans.push_back(-1);
            else ans.push_back(it->second);
        }
        return ans;
    }
};
```



