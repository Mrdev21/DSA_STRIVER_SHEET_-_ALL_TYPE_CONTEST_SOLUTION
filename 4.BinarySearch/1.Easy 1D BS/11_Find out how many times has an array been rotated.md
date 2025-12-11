
# Find number of rotations — Brute Approach

## 🧠 Intuition (Hinglish)
Brute idea simple hai: poora array scan karo aur **smallest element** dhundo.  
Jis index par smallest element milega, wahi rotation count hota hai (because original sorted array rotated `k` times moves min from index 0 to index k).  
Agar multiple equal minima (duplicates), return first occurrence index.

Time: O(n), Space: O(1). Always correct.

---

## 📝 Dry Run
arr = [4,5,6,7,0,1,2]  
Scan: min = 0 at index 4 → rotations = **4**

arr already sorted: [1,2,3,4] → min at index 0 → rotations = **0**

---

## 💻 Code
```java
// Brute: find index of minimum by linear scan
public static int rotationCountBrute(int[] arr) {
    if (arr == null || arr.length == 0) return 0;
    int minIdx = 0;
    for (int i = 1; i < arr.length; i++) {
        if (arr[i] < arr[minIdx]) minIdx = i;
    }
    return minIdx; // number of rotations
}
```


# Find number of rotations — Better Approach

## 🧠 Intuition (Hinglish)
Better approach: scan and look for the **pivot point** where `arr[i] > arr[i+1]`.  
Pivot point `i` indicates minimum at `i+1`, so rotations = `i+1`.  
Agar aisa pair na mile → array not rotated → rotations = 0.

Time: O(n) but often stops early when rotation break found. Space O(1).

---

## 📝 Dry Run
arr = [7,8,9,1,2,3,4]  
i=0: 7<8  
i=1: 8<9  
i=2: 9>1 → pivot i=2 → min at 3 → rotations = **3**

arr sorted: [1,2,3,4] → no pivot → rotations = **0**

---

## 💻 Code
```java
// Better: find first drop arr[i] > arr[i+1]
public static int rotationCountBetter(int[] arr) {
    if (arr == null || arr.length == 0) return 0;
    for (int i = 0; i < arr.length - 1; i++) {
        if (arr[i] > arr[i + 1]) {
            return i + 1; // index of minimum
        }
    }
    return 0; // no rotation
}
```



# Find number of rotations — Optimal Approach (Binary Search, O(log n))

## 🧠 Intuition (Hinglish)
Optimal standard trick: binary search to find **index of minimum (pivot)** in rotated sorted array (no duplicates).  
Key observations:
- If `arr[low] <= arr[high]` → array already sorted → min at `low` → rotations = low (usually 0).
- Compute `mid`. If `arr[mid] > arr[high]` → minimum is to the **right** of mid → `low = mid + 1`.
- Else (`arr[mid] <= arr[high]`) → minimum is at mid or to the **left** → `high = mid`.
Loop ends when `low == high` → that's pivot index → return it.

Time: O(log n), Space: O(1).

**Note about duplicates:** if duplicates allowed (arr[mid] == arr[high]) you must shrink search (`high--`) which may degrade worst-case to O(n). See comment below.

---

## 📝 Dry Run
arr = [4,5,6,7,0,1,2]
low=0, high=6
mid=3 -> arr[3]=7 > arr[6]=2 -> low = mid+1 = 4
low=4, high=6
mid=5 -> arr[5]=1 <= arr[6]=2 -> high = mid = 5
low=4, high=5
mid=4 -> arr[4]=0 <= arr[5]=1 -> high = mid = 4
low==high==4 -> pivot = 4 -> rotations = **4**

arr sorted: [1,2,3,4] -> arr[low] <= arr[high] true -> return low = 0 -> rotations = **0**

---

## 💻 Code
```java
// Optimal: binary search for pivot (works for arrays without duplicates)
public static int rotationCountOptimal(int[] arr) {
    if (arr == null || arr.length == 0) return 0;

    int low = 0, high = arr.length - 1;

    // if already sorted (no rotation)
    if (arr[low] <= arr[high]) return low; // usually 0

    while (low < high) {
        int mid = low + (high - low) / 2;

        if (arr[mid] > arr[high]) {
            // min is in right half
            low = mid + 1;
        } else {
            // min is at mid or in left half
            high = mid;
        }
    }

    return low; // pivot index = number of rotations
}
```

**Duplicate-safe variant note (may degrade to O(n)):**
If array may contain duplicates, modify the loop as follows:

```java
while (low < high) {
    int mid = low + (high - low) / 2;
    if (arr[mid] > arr[high]) {
        low = mid + 1;
    } else if (arr[mid] < arr[high]) {
        high = mid;
    } else { // arr[mid] == arr[high]
        // cannot decide which side; shrink high
        high--;
    }
}
return low;
```

This handles duplicates correctly but in worst-case (many equal values) complexity becomes O(n).



