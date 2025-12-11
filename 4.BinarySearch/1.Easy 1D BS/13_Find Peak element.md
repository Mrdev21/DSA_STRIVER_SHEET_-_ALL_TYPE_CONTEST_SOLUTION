


# Find Peak Element (Brute Approach)

## 🧠 Intuition (Hinglish)
Brute force me simple idea: har index i ke liye check karo ke kya `arr[i]` dono neighbours se bada hai (`arr[i] > arr[i-1]` and `arr[i] > arr[i+1]`).  
Boundary elements ko special treat karo: first element peak ho sakta hai agar `arr[0] > arr[1]`; last element bhi agar `arr[n-1] > arr[n-2]`.  
Sortedness ka koi use nahi — direct scan. Time O(n).

---

## 📝 Dry Run
arr = [1, 3, 2, 1]

i=0 -> compare with right: 1 > 3? no  
i=1 -> 3 > 1 (left) and 3 > 2 (right) → **peak at index 1** -> return 1

---

## 💻 Code
```java
// Brute: O(n) time, check neighbors directly
public static int findPeakBrute(int[] arr) {
    int n = arr.length;
    if (n == 0) return -1;
    if (n == 1) return 0;

    // check first
    if (arr[0] > arr[1]) return 0;

    // check middle elements
    for (int i = 1; i < n - 1; i++) {
        if (arr[i] > arr[i - 1] && arr[i] > arr[i + 1]) return i;
    }

    // check last
    if (arr[n - 1] > arr[n - 2]) return n - 1;

    return -1; // theoretically won't happen if array guaranteed to have a peak
}
```



# Find Peak Element (Better Approach — Single pass greedy)

## 🧠 Intuition (Hinglish)
Better approach bhi O(n) hi hai but thoda simpler: array me jab bhi `arr[i] < arr[i+1]` mile to peak is on right side; jab `arr[i] > arr[i+1]` mile to `i` (or left side) may contain peak.  
Ek straight single pass: jab first time `arr[i] > arr[i+1]` mile, `i` is a local peak (actually `i` is peak of that descending point, so return `i`).  
If never a drop (strictly increasing whole array) → last index is peak.  
Time O(n), code concise.

---

## 📝 Dry Run
arr = [1,2,3,4]

Traverse:
1<2 → continue  
2<3 → continue  
3<4 → continue  
no drop found → array strictly increasing → **peak = index 3**

Alternative example:
arr = [1,3,2]
i=0: 1<3 continue
i=1: 3>2 -> drop found at i=1 -> **peak = 1**

---

## 💻 Code
```java
// Better: single-pass greedy O(n)
public static int findPeakBetter(int[] arr) {
    int n = arr.length;
    if (n == 0) return -1;
    for (int i = 0; i < n - 1; i++) {
        if (arr[i] > arr[i + 1]) {
            return i; // found drop, i is a peak
        }
    }
    return n - 1; // no drop => strictly increasing => last index is peak
}
```



# Find Peak Element (Optimal Approach — Binary Search, O(log n))

## 🧠 Intuition (Hinglish)
Optimal trick: use binary search on the *slope*.  
Compare `arr[mid]` with `arr[mid+1]`:
- agar `arr[mid] < arr[mid+1]` → slope is ascending at mid → peak must be to the **right** of mid → `low = mid + 1`
- agar `arr[mid] > arr[mid+1]` → slope is descending at mid → peak is at `mid` or **left** → `high = mid`
Loop until `low == high`; woh index kisi peak par hi rahega.  
Works because any array has at least one peak (local maxima or boundary), and this method converges to one of them. Time O(log n), space O(1).

---

## 📝 Dry Run
arr = [1, 3, 2, 1]
low=0, high=3
mid=1 -> arr[1]=3, arr[2]=2 -> arr[mid] > arr[mid+1] -> high = mid = 1
low=0, high=1
mid=0 -> arr[0]=1, arr[1]=3 -> arr[mid] < arr[mid+1] -> low = mid+1 = 1
low==high==1 -> **peak = 1**

Another: arr = [1,2,3,4]
low=0,high=3
mid=1 -> 2<3 -> low=2
mid=2 -> 3<4 -> low=3
low==high==3 -> peak index 3 (last)

---

## 💻 Code
```java
// Optimal: binary search on slope. O(log n)
public static int findPeakOptimal(int[] arr) {
    int n = arr.length;
    if (n == 0) return -1;
    int low = 0, high = n - 1;

    while (low < high) {
        int mid = low + (high - low) / 2;
        if (arr[mid] < arr[mid + 1]) {
            // ascending slope -> peak on right
            low = mid + 1;
        } else {
            // descending slope or peak at mid -> peak on left (including mid)
            high = mid;
        }
    }
    // low == high => index of a peak
    return low;
}
```


