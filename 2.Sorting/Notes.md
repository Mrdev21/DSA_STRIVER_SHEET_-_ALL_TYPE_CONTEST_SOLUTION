
## ✅ Quick Comparison Table

| Algorithm | Time (Best/Avg/Worst) | Space | Stable | In-place |
|-----------|------------------------|-------|--------|----------|
| Bubble | O(n)/O(n²)/O(n²) | O(1) | Yes | Yes |
| Selection | O(n²)/O(n²)/O(n²) | O(1) | No | Yes |
| Insertion | O(n)/O(n²)/O(n²) | O(1) | Yes | Yes |
| Merge | O(n log n) all | O(n) | Yes | No (needs aux) |
| Quick (avg) | O(n log n)/O(n²) | O(log n) recursion | No | Yes |
| Heap | O(n log n) | O(1) | No | Yes |
| Counting | O(n + k) | O(n + k) | Yes | No |
| Radix | O(d*(n+b)) | O(n + b) | Yes | No |
| Bucket | Avg O(n + k) | O(n + k) | Often | No |
| Shell | depends | O(1) | No | Yes |

---

## 🔍 Practical Notes & Tips (Hinglish)
- **Use built-in sorts** (Arrays.sort) for production — they are highly optimized (Dual-Pivot QuickSort for primitives in Java, TimSort for objects).  
- **Stable vs In-place**: stable important if you rely on original order for equal keys.  
- **Counting / Radix** are non-comparison sorts and beat O(n log n) when keys small.  
- **QuickSort** with random pivot or median-of-three avoids pathological O(n²).  
- **MergeSort** is the go-to stable O(n log n).  
- **HeapSort** gives guaranteed O(n log n) with O(1) extra space but not stable.  
- For interview: demonstrate understanding of when to use which sort and mention complexities and stability.

---

## 🧾 References (short)
- Know how built-in `Arrays.sort()` behaves: primitives → dual-pivot quicksort (not stable), objects → TimSort (stable).  
- For Big Data / external sorting use variants of merge/external merge sort.

---
