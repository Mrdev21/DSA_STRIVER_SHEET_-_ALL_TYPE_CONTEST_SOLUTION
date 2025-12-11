
### 🧠 Intuition (Hinglish)
Har pass me **smallest element pick** karo aur use current index pe place karo — so smallest elements “select” ho ke left side me collect hote hain.

### ⚙️ Approach
- For i from 0..n-2: find index minIdx of min element in arr[i..n-1]; swap arr[i], arr[minIdx].

### 📝 Dry Run
arr = [4,2,3,1]
i=0: min=1 idx=3 → [1,2,3,4]
i=1: min=2 idx=1 → unchanged etc.

### 💻 Code
```java
public static void selectionSort(int[] arr) {
    int n = arr.length;
    for (int i = 0; i < n - 1; i++) {
        int minIdx = i;
        for (int j = i + 1; j < n; j++) {
            if (arr[j] < arr[minIdx]) minIdx = j;
        }
        int t = arr[i]; arr[i] = arr[minIdx]; arr[minIdx] = t;
    }
}
```

### ⏱ Complexity
- Time: O(n²) always  
- Space: O(1)  
- Stable: No (by default), can be made stable but costly  
- Use: small memory footprint, simple.

---
