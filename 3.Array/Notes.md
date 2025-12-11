

# 📘 Arrays — Complete A to Z Theory Notes

---

## 1️⃣ What is an Array?
Array ek **linear, contiguous, fixed-size data structure** hota hai jisme same type ke elements store hote hain aur unka indexing fixed hota hai.

- Memory me elements **back-to-back** store hote hain.
- Fast random access → O(1)
- Size **fixed** hota hai once created (Java arrays).
- Index range: **0 to n−1**

---

## 2️⃣ Why Use Arrays?
- Fast read operations → O(1)
- Easy to iterate
- Best for storing **lists of similar data**
- Low overhead (contiguous memory)

---

## 3️⃣ Array Representation in Memory

```
Index:  0   1   2   3   4
Value: 10  20  30  40  50
Address: base + i * size_of(datatype)
```

If base = 1000, int = 4 bytes:

```
1000 → 10
1004 → 20
1008 → 30
1012 → 40
1016 → 50
```

---

## 4️⃣ Basic Operations on Arrays

### ✔ Access (Read)
Time: **O(1)**  
`arr[i]` → direct index mapping → very fast.

### ✔ Update
Time: **O(1)**  
`arr[i] = newValue`

### ✔ Insertion
- At end → O(1)
- At index → O(n) (shift required)

### ✔ Deletion
- From end → O(1)
- From index → O(n)

---

## 5️⃣ Time Complexity Summary Table

| Operation | Best | Worst |
|----------|------|--------|
| Access | O(1) | O(1) |
| Search (unsorted) | O(1) | O(n) |
| Search (sorted + binary search) | O(log n) | O(log n) |
| Insert at index | O(n) | O(n) |
| Delete at index | O(n) | O(n) |
| Traverse | O(n) | O(n) |

---

## 6️⃣ Types of Arrays

### 1. Static Array  
- Fixed size  
- Stored in contiguous memory  

### 2. Dynamic Array (ArrayList in Java)  
- Resize automatically  
- Amortized O(1) insertion at end  
- Internally still uses static array

### 3. Multi-dimensional Arrays  
- 2D Arrays → Matrix  
- 3D Arrays → Tensors  
- Used for DP, Graphs, Matrices

### 4. Jagged Arrays  
- Array of arrays with different lengths  

---

## 7️⃣ Array Patterns (Most Important For DSA)

### ⭐ 1. Two-pointer Technique  
Used when array sorted / or we maintain two ends.

Applications:
- 2-sum sorted
- Move zeros
- Reverse array
- Merge two sorted arrays

---

### ⭐ 2. Sliding Window  
Used when subarray-based problems:

Types:
- Fixed window  
- Variable window  

Applications:
- Max subarray sum (size k)
- Longest substring without repeating characters
- Count subarrays with sum ≥ K

---

### ⭐ 3. Prefix Sum  
Used to compute range-sum queries fast.

Formula:
```
prefix[i] = prefix[i-1] + arr[i]
sum(l, r) = prefix[r] - prefix[l-1]
```

Applications:
- Subarray sum problems
- Equilibrium index
- Count subarrays with given sum

---

### ⭐ 4. Hashing / Frequency Counting  
Used when duplicates / frequency / set operations needed.

Applications:
- Majority element
- Count occurrences
- Union / intersection
- Repeating + missing number

---

### ⭐ 5. Binary Search on Sorted Arrays  
Use when array is sorted OR answer monotonic hota hai.

Applications:
- First/last occurrence
- Search in rotated array
- Peak element
- Search insert position
- Binary search on answer (capacity to ship packages, aggressive cows)

---

## 8️⃣ Important Array Interview Concepts

### ✔ Left & Right Rotation  
Rotate array by k positions (mod n)

### ✔ Rearrangement  
- Partition negative/positive
- Sort colors (0,1,2)
- Alternate positive/negative

### ✔ Subarray Problems  
- Kadane’s Algorithm  
- Maximum product subarray  
- Longest subarray with sum K  
- Sliding window variants  

### ✔ Searching Patterns  
- Binary search  
- Pivot search  
- Lower/Upper bound  

### ✔ Sorting Techniques  
- Bubble, Selection, Insertion  
- Merge sort  
- Quick sort  

---

## 9️⃣ When to Choose Arrays Over Other DS?

Use arrays when:
- Need fast random access O(1)
- Size is fixed / known
- Insertion-deletion in middle not frequent
- Cache-friendliness required
- DP tables / matrices needed

---

## 🔟 Pros & Cons of Arrays

### 👍 Advantages
- Fast access (O(1))
- Predictable memory layout
- Easy implementation
- Good for mathematical/DP problems

### 👎 Disadvantages
- Fixed size (for static arrays)
- Insert/delete at index = expensive
- Large continuous memory required
- Won’t handle dynamic growth without reallocation

---

## 1️⃣1️⃣ Common Pitfalls / Edge Cases

- Forgetting **0-based indexing**
- Array index out of bounds
- Overflow when summing large arrays
- Using wrong window size in sliding window
- Forgetting to use modulo for rotations
- Wrong pivot in rotated search
- Integer overflow when doing `2 * arr[i]` etc.

---

## 1️⃣2️⃣ High-level Interview Summary

“Array problems mostly revolve around patterns:  
- **Two pointers** (sorted arrays / partitioning)  
- **Sliding window** (subarray constraints)  
- **Binary search** (sorted or answer search)  
- **Prefix sum** (subarray sum logic)  
- **Hashing** (frequency or existence check)  
- **Sorting** (placing items in order)  

If you master these, 70% array questions become intuitive.”

---

## 1️⃣3️⃣ Diagram: Search vs Insert vs Delete (Big Picture)

```
          Access: O(1)
           |
Insert ----+---- Delete
   |                 |
 O(n)              O(n)
```

---

## 1️⃣4️⃣ Most Important Formulas

Rotation:
```
k = k % n
```

Mid formula (safe):
```
mid = low + (high - low) / 2
```

Kadane:
```
cur = max(arr[i], cur + arr[i])
best = max(best, cur)
```

Prefix sum:
```
prefix[i] = prefix[i-1] + arr[i]
```

---

## 1️⃣5️⃣ Must-Know Array Problems (Interview Checklist)

- Two Sum  
- Majority Element  
- Kadane’s Algorithm  
- Maximum Product Subarray  
- Move Zeroes  
- Rotate Array  
- Binary Search problems  
- Sliding Window max  
- Trapping Rainwater  
- Merge Intervals  
- Pascals Triangle  
- Dutch National Flag (012 Sort)

---

## ✅ Final Summary (One-liner)
Arrays are fast, simple, cache-friendly structures that form the foundation of **two-pointer**, **binary search**, **DP**, **sliding window**, and **prefix sum** techniques.

