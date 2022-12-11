703.Kth Largest Element in a Stream
第 k 大 -> greater int

```cpp
class KthLargest {
public:
    priority_queue<int, vector<int>, greater<int>> pq;
    int size;
    KthLargest(int k, vector<int>& nums) {
        // pq = priority_queue<int>(nums.begin(), nums.end());
        size = k;
        for(auto &u: nums) pq.push(u);
        // pq.resize(nums.begin(), nums.end());
        while(pq.size() > size) pq.pop();
    }
    int add(int val) {
        pq.push(val);
        while(pq.size() > size) pq.pop();
        return pq.top();
    }
};
```

