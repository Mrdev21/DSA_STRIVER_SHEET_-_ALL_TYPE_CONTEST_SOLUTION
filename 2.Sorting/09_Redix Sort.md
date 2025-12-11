### 🧠 Intuition
Sort by individual digits from least significant to most significant using stable counting sort per digit.

### ⚙️ Approach
- For d = 0..maxDigits-1:
  - stable counting sort by digit `(arr[i]/10^d) % 10`.

### 📝 Dry Run
arr=[170,45,75,90,802,24,2,66]
sort by units → tens → hundreds → final sorted.

### 💻 Code
```java
public static int[] radixSort(int[] arr) {
    int n = arr.length;
    int max = 0;
    for (int v : arr) if (v > max) max = v;
    int exp = 1;
    int[] out = new int[n];
    while (max / exp > 0) {
        int[] count = new int[10];
        for (int i = 0; i < n; i++) count[(arr[i] / exp) % 10]++;
        for (int i = 1; i < 10; i++) count[i] += count[i - 1];
        for (int i = n - 1; i >= 0; i--) {
            int d = (arr[i] / exp) % 10;
            out[count[d] - 1] = arr[i];
            count[d]--;
        }
        System.arraycopy(out, 0, arr, 0, n);
        exp *= 10;
    }
    return arr;
}
```

### ⏱ Complexity
- Time: O(d*(n + b)) where d = digits, b = base (10)  
- Space: O(n + b)  
- Stable: Yes (uses stable counting)  
- Use: large arrays of integers with bounded digits.

---
