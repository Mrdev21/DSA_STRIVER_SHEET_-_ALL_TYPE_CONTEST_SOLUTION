
# Search in Rotated Sorted Array I (Brute Approach)

## 🧠 Intuition (Hinglish)
Brute approach me simple linear scan karo — har element check karo agar `arr[i] == target` to index return.  
Rotation ya sortedness ka koi use nahi hota — lekin guaranteed correct aur easiest.

Time: O(n), Space: O(1)

---

## 📝 Dry Run
arr = [4,5,6,7,0,1,2], target = 0

i=0 → 4  
i=1 → 5  
i=2 → 6  
i=3 → 7  
i=4 → 0 → found at index **4**

---

## 💻 Code
```java
public static int searchRotatedBrute(int[] arr, int target) {
    if (arr == null) return -1;
    for (int i = 0; i < arr.length; i++) {
        if (arr[i] == target) return i;
    }
    return -1;
}
```


# Search in Rotated Sorted Array I (Better Approach — Find Pivot then Binary Search)

## 🧠 Intuition (Hinglish)
Better approach: pehle rotation ka **pivot** (smallest element ka index) binary-search se O(log n) me find karo.  
Pivot mil jaaye to array ko do sorted ranges mil jaate hain: `[0..pivot-1]` and `[pivot..n-1]`.  
Phir decide karo target kis segment me ho sakta hai (compare target with arr[0] and arr[n-1]) aur us segment me normal binary search karo.  
Total time O(log n) + O(log n) = O(log n). Space O(1).

---

## 📝 Dry Run
arr = [4,5,6,7,0,1,2], target = 1

Step1: find pivot (index of smallest element) → pivot = 4 (value 0)  
Since target (1) >= arr[pivot] (0) and <= arr[n-1] (2) → search in [pivot..n-1] = [0..2]  
Binary search in that segment finds `1` at index **5**

---

## 💻 Code
```java
// helper: find index of smallest element (pivot) in rotated sorted array (no duplicates)
public static int findPivot(int[] arr) {
    int n = arr.length;
    int low = 0, high = n - 1;

    while (low < high) {
        int mid = low + (high - low) / 2;
        if (arr[mid] > arr[high]) {
            // pivot is to the right
            low = mid + 1;
        } else {
            // pivot is at mid or to the left
            high = mid;
        }
    }
    return low; // pivot index
}

public static int binarySearch(int[] arr, int l, int r, int target) {
    int low = l, high = r;
    while (low <= high) {
        int mid = low + (high - low) / 2;
        if (arr[mid] == target) return mid;
        else if (arr[mid] < target) low = mid + 1;
        else high = mid - 1;
    }
    return -1;
}

public static int searchRotatedBetter(int[] arr, int target) {
    if (arr == null || arr.length == 0) return -1;
    int n = arr.length;
    int pivot = findPivot(arr);

    // if not rotated (pivot == 0), just binary search whole array
    if (pivot == 0) return binarySearch(arr, 0, n - 1, target);

    if (target >= arr[pivot] && target <= arr[n - 1]) {
        return binarySearch(arr, pivot, n - 1, target);
    } else {
        return binarySearch(arr, 0, pivot - 1, target);
    }
}
```


# Search in Rotated Sorted Array I (Optimal Approach — Modified Binary Search in one pass)

## 🧠 Intuition (Hinglish)
Classic optimal trick: ek hi modified binary search me decide karo ki **kaunsa half sorted hai**, aur us sorted half me target ho sakta hai ya nahi:
- mid compute karo
- agar `arr[mid] == target` → return mid
- agar left half sorted (`arr[low] <= arr[mid]`):
  - agar target in `[arr[low], arr[mid])` → move `high = mid - 1` (search left)
  - else `low = mid + 1` (search right)
- else (right half sorted):
  - agar target in `(arr[mid], arr[high]]` → `low = mid + 1`
  - else `high = mid - 1`

Yeh approach single-pass O(log n) aur space O(1) deta hai — interview standard.

**Note:** works when no duplicates. With duplicates you'd need more careful handling.

---

## 📝 Dry Run
arr = [4,5,6,7,0,1,2], target = 0

low=0, high=6  
mid=3 → arr[mid]=7, arr[low]=4 → left half [4..7] sorted  
target=0 not in [4..7] → search right → low=4

low=4, high=6  
mid=5 → arr[mid]=1, left half [0..1] sorted (arr[4]=0 <= arr[5]=1)  
target=0 in [0..1] → search left → high=4

low=4, high=4  
mid=4 → arr[4]=0 → FOUND at index **4**

---

## 💻 Code
```java
public static int searchRotatedOptimal(int[] arr, int target) {
    if (arr == null || arr.length == 0) return -1;
    int low = 0, high = arr.length - 1;

    while (low <= high) {
        int mid = low + (high - low) / 2;
        if (arr[mid] == target) return mid;

        // left half sorted?
        if (arr[low] <= arr[mid]) {
            if (target >= arr[low] && target < arr[mid]) {
                high = mid - 1; // target in left sorted half
            } else {
                low = mid + 1;
            }
        } else {
            // right half must be sorted
            if (target > arr[mid] && target <= arr[high]) {
                low = mid + 1; // target in right sorted half
            } else {
                high = mid - 1;
            }
        }
    }

    return -1; // not found
}
```

