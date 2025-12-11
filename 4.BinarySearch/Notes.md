
# 🔍 Binary Search — Complete A to Z Notes

---

## 1️⃣ What is Binary Search?
Binary Search ek algorithm hai jo **sorted array/list** me search karne ke liye use hota hai.

Idea:
- Array ko bar-bar **half** me divide karo  
- Decide karo answer **left half** me hoga ya **right half** me  
- Time complexity reduce ho jati hai O(log n)

---

## 2️⃣ Prerequisites (Most Important)
Binary Search **tabhi kaam karega** jab:

✔ Array **sorted** ho  
✔ OR f(x) (answer space) **monotonic** ho  
✔ Random access possible ho (array, not linked list)

---

## 3️⃣ Why Binary Search?
Linear search = O(n)  
Binary search = **O(log n)** → extremely fast

Example:  
n=1,000,000 → Binary Search max 20 steps me answer.

---

## 4️⃣ How Does Binary Search Work?

Algorithm:
1. low = 0  
2. high = n - 1  
3. mid = low + (high - low) / 2  
4. Compare arr[mid] with target  
5. Decide left or right  
6. Repeat until low > high

---

## 5️⃣ Safe Mid Formula

**Wrong:**  
```
mid = (low + high) / 2  // overflow risk
```

**Correct:**  
```
mid = low + (high - low) / 2
```

---

## 6️⃣ Standard Binary Search Template

```
while (low <= high):
    mid = low + (high - low) // 2

    if arr[mid] == target:
        return mid

    if arr[mid] < target:
        low = mid + 1
    else:
        high = mid - 1
return -1
```

---

## 7️⃣ Boundary Conditions
Binary search ka sabse tricky part:  
- low <= high  
- mid calculation  
- updating low/high correctly  
- infinite loops avoid karna

---

## 8️⃣ Time & Space Complexity

| Step | Complexity |
|------|------------|
| Each iteration | O(1) |
| Total iterations | O(log n) |
| Space | O(1) |

---

## 9️⃣ Binary Search Variants (The Real Interview Meat)

### ⭐ 1. Lower Bound  
Find first index `i` such that `arr[i] >= x`.

### ⭐ 2. Upper Bound  
Find first index `i` such that `arr[i] > x`.

### ⭐ 3. First Occurrence  
First index where `arr[i] == x`.

### ⭐ 4. Last Occurrence  
Last index where `arr[i] == x`.

### ⭐ 5. Search Insert Position  
Where x should be inserted to keep array sorted.

### ⭐ 6. Binary Search on Answer (Advanced)  
When array sorted **nahi hota** but answer monotonic hota hai.

Used in:
- Aggressive Cows  
- Allocate Books  
- Koko Eating Bananas  
- Minimum days to make bouquets  
- Capacity to Ship Packages  
- Split Array Largest Sum  

**Idea:**
1. Low = smallest possible answer  
2. High = largest possible answer  
3. Mid = candidate answer  
4. Check feasibility(mid)  
5. If feasible → right me try (high = mid)  
6. Else → left me jao (low = mid+1)

---

## 🔥 10️⃣ Searching in Rotated Sorted Array
### Case I: No duplicates  
Use condition:
```
if(arr[low] <= arr[mid]) → left sorted
else → right sorted
```

### Case II: Duplicates exist  
If `arr[low] == arr[mid]`, shrink search space (`low++`).

---

## 🔥 1️⃣1️⃣ Peak Element Search  
Use slope technique:
```
if arr[mid] < arr[mid+1] → right me jao
else → left me jao
```

---

## 1️⃣2️⃣ Binary Search on Real Numbers (Float BS)
Used when precision required:  
- Square root  
- nth root  
- Pie cutting  
- Allocate angles etc.

Stops when:
```
high - low < 1e-9
```

---

## 1️⃣3️⃣ When Binary Search Fails
❌ Array sorted nahi hai  
❌ Condition monotonic nahi hai  
❌ Mid update wrong  
❌ Infinite loop  
❌ Off-by-one errors  
❌ Overflow in mid formula (int overflow)

---

## 1️⃣4️⃣ Patterns to Identify Binary Search Problems

| Prompt | Possible Pattern |
|--------|------------------|
| “Find smallest/largest …” | BS on answer |
| “Return first/last…” | Bound search |
| “Search in rotated…” | Pivot logic |
| “Find peak/valley” | Slope-based BS |
| “Minimize/maximize …” | Decision-based BS |
| “Given k, find min X such that condition holds” | Predicate monotonic |

---

## 1️⃣5️⃣ Visual Understanding (Peak Example)

```
arr = [1,2,3,1]

    3    ← peak
   / \
  2   1
 /
1
```

Binary search slope logic works because:
- If mid on rising slope → peak right me  
- If mid on falling slope → peak left me

---

## 1️⃣6️⃣ Real-World Use Cases

- Game development (collision detection)  
- Compiler optimization  
- Database indexing  
- Search engines  
- Auto-suggestion systems  
- Scheduling algorithms  

---

## 1️⃣7️⃣ Checklist To Avoid Mistakes

✔ mid = low + (high−low)/2  
✔ Check sorted half carefully  
✔ Don’t use >= in lower bound  
✔ Don’t use > in upper bound incorrectly  
✔ Rotated sorted array me equal case handle karo  
✔ Infinite loop avoid (low = mid+1 / high = mid-1 mandatory)  

---

## 1️⃣8️⃣ Edge Cases

- Array length = 1  
- All values same (duplicates)  
- Target < arr[0]  
- Target > arr[n-1]  
- Rotation by n or 0  
- Very large values (overflow)

---

## 1️⃣9️⃣ Ultimate Binary Search One-Line Summary

“Binary Search finds answers in **logarithmic steps** by narrowing search space based on a **monotonic condition**.”

