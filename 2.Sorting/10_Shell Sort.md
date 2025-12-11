
### 🧠 Intuition
Generalized insertion sort — first sort elements far apart using a gap, gradually reduce gap to 1. This reduces large shifts.

### ⚙️ Approach
- Choose gap sequence (e.g., n/2, n/4, ..., 1 or better sequences).
- For each gap: do gapped insertion sort.

### 📝 Dry Run
arr=[8,5,3,7,6,2,4,1], gap=4 → compare index pairs distance 4, then gap=2 etc → final insertion sort with gap=1.

### 💻 Code
```java
public static void shellSort(int[] arr) {
    int n = arr.length;
    for (int gap = n / 2; gap > 0; gap /= 2) {
        for (int i = gap; i < n; i++) {
            int temp = arr[i];
            int j = i;
            while (j >= gap && arr[j - gap] > temp) {
                arr[j] = arr[j - gap];
                j -= gap;
            }
            arr[j] = temp;
        }
    }
}
```

### ⏱ Complexity
- Depends on gap sequence — worst-case between O(n log² n) and O(n²).  
- Space: O(1)  
- Stable: No  
- Use: simple improvement over insertion for medium sized arrays.

---