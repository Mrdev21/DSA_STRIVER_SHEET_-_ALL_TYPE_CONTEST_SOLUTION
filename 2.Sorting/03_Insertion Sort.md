### 🧠 Intuition (Hinglish)
Array ko left se sorted grow karte ho. Har new element ko correct position pe insert karo by shifting larger elements right — jaise cards ko sorted rakhna.

### ⚙️ Approach
- For i from 1..n-1: key = arr[i]; j=i-1; shift while arr[j] > key; place key at j+1.

### 📝 Dry Run
arr = [4,2,3,1]
i=1 key=2 → shift 4 → [2,4,3,1]
i=2 key=3 → shift 4 → [2,3,4,1]
i=3 key=1 → shift 4,3,2 → [1,2,3,4]

### 💻 Code
```java
public static void insertionSort(int[] arr) {
    int n = arr.length;
    for (int i = 1; i < n; i++) {
        int key = arr[i];
        int j = i - 1;
        while (j >= 0 && arr[j] > key) {
            arr[j + 1] = arr[j];
            j--;
        }
        arr[j + 1] = key;
    }
}
```

### ⏱ Complexity
- Best: O(n) (already sorted)  
- Avg/Worst: O(n²)  
- Space: O(1)  
- Stable: Yes  
- Use: small arrays, nearly-sorted arrays.

---