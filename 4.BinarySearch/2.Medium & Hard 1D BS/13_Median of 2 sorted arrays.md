
---
**Problem (short):** Given two sorted arrays `nums1[]` and `nums2[]`, find the **median** of the combined sorted array (without actually merging them). Return the median as a double. Arrays sizes can be different (including zero). Aim for `O(log(min(n,m)))` time.

---

# Brute Approach

## 🧠 Intuition (Hinglish)

Brute me dono arrays ko merge karke ek sorted array bana lo, phir median directly nikaal lo.

Simple aur straightforward — lekin extra `O(n+m)` space and `O(n+m)` time.

---

## 📝 Dry Run

nums1 = [1,3], nums2 = [2]

- Merge -> [1,2,3]
    
- Median = 2.0
    

nums1 = [1,2], nums2 = [3,4]

- Merge -> [1,2,3,4]
    
- Median = (2+3)/2 = 2.5
    

---

## 💻 Code (Java)

```java
// Brute: merge two arrays and compute median -> O(n+m) time, O(n+m) space
public static double findMedianBrute(int[] nums1, int[] nums2) {
    int n = nums1.length, m = nums2.length;
    int[] merged = new int[n + m];
    int i = 0, j = 0, k = 0;
    while (i < n && j < m) {
        if (nums1[i] <= nums2[j]) merged[k++] = nums1[i++];
        else merged[k++] = nums2[j++];
    }
    while (i < n) merged[k++] = nums1[i++];
    while (j < m) merged[k++] = nums2[j++];

    int len = n + m;
    if (len % 2 == 1) return merged[len/2];
    else return (merged[len/2 - 1] + merged[len/2]) / 2.0;
}
```

## Complexity

- Time: `O(n + m)`
    
- Space: `O(n + m)`
    

---

# Better Approach

## 🧠 Intuition (Hinglish)

Space optimize karne ke liye **merge-on-the-fly** kar sakte ho but stop when you reach middle. Isse time `O(n+m)` lekin space `O(1)`.

Algorithm: use two pointers and advance until you reach the k-th element(s) needed for median.

---

## 📝 Dry Run

nums1=[1,3], nums2=[2]

- advance pointers until we reach index 1 (0-based) -> elements seen [1,2]
    
- median = 2.0
    

---

## 💻 Code (Java)

```java
// Better: two-pointer merge until median index, O(n+m) time, O(1) extra space
public static double findMedianTwoPointers(int[] nums1, int[] nums2) {
    int n = nums1.length, m = nums2.length;
    int total = n + m;
    int medianIdx1 = (total - 1) / 2; // left middle for even
    int medianIdx2 = total / 2;       // right middle for even (same as medianIdx1 if odd)

    int i = 0, j = 0, count = 0;
    int curr = 0, prev = 0;
    while (count <= medianIdx2) {
        prev = curr;
        if (i < n && (j >= m || nums1[i] <= nums2[j])) {
            curr = nums1[i++];
        } else {
            curr = nums2[j++];
        }
        count++;
    }
    if (total % 2 == 1) return curr;
    else return (prev + curr) / 2.0;
}
```

## Complexity

- Time: `O(n + m)` (but stops early)
    
- Space: `O(1)` extra
    

---

# Optimal Approach (Binary Search Partition)

## 🧠 Intuition (Hinglish)

Optimal trick (classic): binary search partition on the **smaller array**. Partition both arrays such that:

- Left parts contain `half = (n + m + 1) / 2` elements in total.
    
- All elements in left parts `<=` all elements in right parts.
    

Then median is max(leftMax) or average of max(leftMax) and min(rightMin) depending on odd/even total length.

We binary search on partition index `i` in `nums1` and derive `j = half - i` for `nums2`. Check boundary conditions using `-INF`/`+INF` for empty sides.

This gives `O(log(min(n,m)))` time.

---

## 📝 Dry Run

nums1=[1,3], nums2=[2]

- n=2,m=1, half=(3+1)/2=2
    
- binary search i in [0..2]
    
- pick i=1 -> j=1 -> leftMax = max(nums1[0], nums2[0]) = max(1,2)=2; rightMin = min(nums1[1], +INF) = 3
    
- total odd -> median = leftMax = 2.0
    

nums1=[1,2], nums2=[3,4]

- n=2,m=2, half=2
    
- i=1 -> j=1 -> leftMax = max(1,3)=3? adjust partitions until leftMax<=rightMin
    
- correct partition leads to leftMax=2, rightMin=3 -> median=(2+3)/2=2.5
    

---

## 💻 Code (Java)

```java
// Optimal: O(log(min(n,m))) time
public static double findMedianOptimal(int[] nums1, int[] nums2) {
    if (nums1.length > nums2.length) return findMedianOptimal(nums2, nums1); // ensure nums1 smaller

    int n = nums1.length, m = nums2.length;
    int low = 0, high = n;
    int half = (n + m + 1) / 2;

    while (low <= high) {
        int i = low + (high - low) / 2;
        int j = half - i;

        int left1 = (i == 0) ? Integer.MIN_VALUE : nums1[i - 1];
        int right1 = (i == n) ? Integer.MAX_VALUE : nums1[i];
        int left2 = (j == 0) ? Integer.MIN_VALUE : nums2[j - 1];
        int right2 = (j == m) ? Integer.MAX_VALUE : nums2[j];

        if (left1 <= right2 && left2 <= right1) {
            // correct partition
            if ((n + m) % 2 == 1) {
                return Math.max(left1, left2);
            } else {
                return (Math.max(left1, left2) + Math.min(right1, right2)) / 2.0;
            }
        } else if (left1 > right2) {
            // too far right in nums1, move left
            high = i - 1;
        } else {
            // left2 > right1 -> too far left in nums1, move right
            low = i + 1;
        }
    }
    throw new IllegalArgumentException("Input arrays not sorted or invalid");
}
```

## Complexity

- Time: `O(log(min(n,m)))`
    
- Space: `O(1)`
    

---

# ✅ Summary / Tips

- If you need simpler code and arrays small → merge or two-pointer approach fine.
    
- For strict `O(log min(n,m))` → use partition binary search.
    
- Handle empty array edge cases carefully (use `MIN_VALUE/MAX_VALUE` as sentinels).
    

---

# Examples

- nums1=[1,3], nums2=[2] → 2.0
    
- nums1=[1,2], nums2=[3,4] → 2.5
    

---

_File placement suggestion:_ `DSA/Arrays/13_Median of Two Sorted Arrays.md`