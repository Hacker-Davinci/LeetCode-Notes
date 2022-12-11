1.Median of Two Sorted Arrays  
pass 

2.Find First and Last Position of Element in Sorted Array  
AC
3.Merge Intervals  
AC
4.Insert Interval  
AC

5.Valid Anagram  
```cpp
class Solution {
public:
    bool isAnagram(string s, string t) {
        map<char, int> before;
        map<char, int> after;
        for(auto &u: s)
            before[u]++;
        for(auto &u: t)
            after[u]++;
        if(before.size() != after.size()) return false;
        for(auto &u: s){
            if(before[u] != after[u]) return false;
        }
        return true;
    }
};
```

6.Count of Smaller Numbers After Self 
pass

7.Peak Index in a Mountain Array
看山還是山
![[Pasted image 20221211131537.png]]

```cpp
class Solution {
public:
	一定有解，所以 r 可以去掉最右邊第一個 element
    int peakIndexInMountainArray(vector<int>& arr) {
        int l = 1, r = arr.size()-2, mid;
        while(l < r){
            mid = l + (r-l) / 2;
            if(arr[mid] > arr[mid+1]) r = mid;
            else l = mid+1;
        }
        return l;
    }
};
```


