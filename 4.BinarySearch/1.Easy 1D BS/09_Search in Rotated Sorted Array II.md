
# Search in Rotated Sorted Array II — Brute Approach (Linear Scan)

## 🧠 Intuition (Hinglish)
Array rotated hai aur duplicates ho sakte hain — easiest aur foolproof way simple **linear scan** hai: sab elements check karo, agar `arr[i] == target` mil gaya → true.  
Guaranteed correct, time O(n), space O(1).  
Duplicates ke wajah se binary-search tricks kabhi ambiguous ho sakti hain — brute safe hai.

---

## 📝 Dry Run
arr = [2,5,6,0,0,1,2], target = 0

i=0 → 2  
i=1 → 5  
i=2 → 6  
i=3 → 0 → found → return **true**

(Linear scan se seedha mil gaya.)

---

## 💻 Code
```java
public static boolean searchRotatedWithDupBrute(int[] arr, int target) {
    if (arr == null) return false;
    for (int v : arr) {
        if (v == target) return true;
    }
    return false;
}
```


# Search in Rotated Sorted Array II — Better Approach (Find Pivot with duplicate-aware method → Binary Search)

## 🧠 Intuition (Hinglish)
Thoda better approach me pehle **pivot (index of smallest element)** find kar lo — par duplicates hain to pivot-finding ko duplicate-safe banana padega:
- Use modified pivot search: compare `arr[mid]` with `arr[high]`.
  - agar `arr[mid] < arr[high]` → pivot left side (including mid)
  - agar `arr[mid] > arr[high]` → pivot right side (mid+1 .. high)
  - agar `arr[mid] == arr[high]` → can't decide → `high--` (this may degrade to O(n) in worst-case)
Pivot milne ke baad array do sorted segments ban jaate hain; decide karo target kis segment me ho sakta hai aur normal binary search karo us segment me.

**Note:** pivot-finding with `high--` is common trick but worst-case O(n) when many duplicates.

---

## 📝 Dry Run
arr = [2,5,6,0,0,1,2], target = 0

Find pivot:
low=0,high=6
mid=3 -> arr[3]=0 <= arr[6]=2 -> high=3
low=0,high=3
mid=1 -> arr[1]=5 > arr[3]=0 -> low=2
low=2,high=3
mid=2 -> arr[2]=6 > arr[3]=0 -> low=3
low=3,high=3 -> pivot=3 (arr[3]=0)

Since target (0) in arr[pivot]..arr[n-1] range → binary search in [3..6] finds 0 at index 3 → **true**

---

## 💻 Code
```java
// find pivot (index of smallest element) in presence of duplicates
public static int findPivotWithDup(int[] arr) {
    int n = arr.length;
    int low = 0, high = n - 1;
    while (low < high) {
        int mid = low + (high - low) / 2;
        if (arr[mid] < arr[high]) {
            high = mid;
        } else if (arr[mid] > arr[high]) {
            low = mid + 1;
        } else { // arr[mid] == arr[high]
            // cannot decide, shrink search space
            high--;
        }
    }
    return low;
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

public static boolean searchRotatedWithDupBetter(int[] arr, int target) {
    if (arr == null || arr.length == 0) return false;
    int n = arr.length;
    int pivot = findPivotWithDup(arr);

    // search in the appropriate half
    if (arr[pivot] == target) return true;
    if (pivot == 0) {
        return binarySearch(arr, 0, n - 1, target) != -1;
    }
    if (target >= arr[0]) {
        // search in left half (0..pivot-1)
        return binarySearch(arr, 0, pivot - 1, target) != -1;
    } else {
        // search in right half (pivot..n-1)
        return binarySearch(arr, pivot, n - 1, target) != -1;
    }
}
```


# Search in Rotated Sorted Array II — Optimal Approach (Modified one-pass binary search that handles duplicates)

## 🧠 Intuition (Hinglish)
Most interview-friendly single-pass method (modified binary search) jo duplicates ko handle karta hai by shrinking when necessary:

Loop while `low <= high`:
1. mid = (low+high)/2. If `arr[mid] == target` → return true.
2. If left half is sorted (`arr[low] < arr[mid]`):
   - if `arr[low] <= target < arr[mid]` → search left (`high = mid - 1`)
   - else search right (`low = mid + 1`)
3. Else if right half is sorted (`arr[mid] < arr[low]`):
   - if `arr[mid] < target <= arr[high]` → search right (`low = mid + 1`)
   - else search left (`high = mid - 1`)
4. Else (when `arr[low] == arr[mid]`) → we cannot determine which half is sorted because of duplicates → do `low++` (shrink left).  
   (You could also do `high--` when `arr[high]==arr[mid]` or both; commonly do `low++`.)

**Complexity note:** This approach typically runs in O(log n) but in worst-case (all elements equal) it degrades to O(n).

---

## 📝 Dry Run (critical duplicate example)
arr = [1, 0, 1, 1, 1], target = 0

low=0, high=4
mid=2 -> arr[mid]=1 != target
arr[low]=1, arr[mid]=1 -> arr[low] == arr[mid], can't decide -> low++ -> low=1

low=1, high=4
mid=2 -> arr[2]=1 != target
now arr[low]=0 < arr[mid]=1 -> left half sorted
check if target in [arr[low], arr[mid]) => 0 in [0,1) -> yes -> search left => high = mid-1 =1

low=1, high=1
mid=1 -> arr[1]=0 == target -> found -> **true**

(This shows how incrementing `low` resolves ambiguity and leads to finding the target.)

---

## 💻 Code
```java
public static boolean searchRotatedWithDupOptimal(int[] arr, int target) {
    if (arr == null || arr.length == 0) return false;

    int low = 0, high = arr.length - 1;
    while (low <= high) {
        int mid = low + (high - low) / 2;
        if (arr[mid] == target) return true;

        // If left half is strictly sorted
        if (arr[low] < arr[mid]) {
            if (target >= arr[low] && target < arr[mid]) {
                high = mid - 1;
            } else {
                low = mid + 1;
            }
        }
        // If right half is strictly sorted
        else if (arr[mid] < arr[low]) {
            if (target > arr[mid] && target <= arr[high]) {
                low = mid + 1;
            } else {
                high = mid - 1;
            }
        }
        // arr[low] == arr[mid] -> duplicates, cannot decide; shrink left
        else {
            low++;
        }
    }

    return false;
}
```

