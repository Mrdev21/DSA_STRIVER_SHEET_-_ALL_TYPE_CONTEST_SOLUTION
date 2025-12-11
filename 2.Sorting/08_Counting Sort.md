
### 🧠 Intuition
If keys are small integers in range [0..k], counting sort counts frequency then computes positions. Very fast O(n + k).

### ⚙️ Approach
- Count frequency in `count[k+1]`.
- Prefix-sum transform counts to positions.
- Place elements into output array using counts (for stable version iterate from right to left).

### 📝 Dry Run
arr=[4,2,2,1,3], k=4
count: [0,1,2,1,1] → prefix → positions → output sorted.

### 💻 Code (stable)
```java
public static int[] countingSort(int[] arr, int k) {
    int n = arr.length;
    int[] count = new int[k + 1];
    for (int v : arr) count[v]++;
    for (int i = 1; i <= k; i++) count[i] += count[i - 1];
    int[] out = new int[n];
    for (int i = n - 1; i >= 0; i--) {
        out[count[arr[i]] - 1] = arr[i];
        count[arr[i]]--;
    }
    return out; // note: returns new array (not in-place)
}
```

### ⏱ Complexity
- Time: O(n + k)  
- Space: O(n + k)  
- Stable: Yes (with right-to-left placement)  
- Use: integers in small range, radix sort building block.

---
