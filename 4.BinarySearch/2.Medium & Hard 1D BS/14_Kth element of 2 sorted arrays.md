**Problem (short, Hinglish):** Tumhe do sorted arrays `nums1[]` aur `nums2[]` di hui hain aur ek integer `k` (1-based). Tumhe combined sorted array (without actually merge kiye) ka **k-th smallest element** return karna hai. Aim hai ki efficient solution ho — ideally `O(log(min(n,m)))` time. Arrays empty bhi ho sakti hain; ensure boundaries handle karo.

# Brute Approach

## 🧠 Intuition (Hinglish)

Brute: dono arrays ko merge kar ke combined sorted array bana lo, fir uska k-th element return karo (1-based k). Simple and straightforward but uses extra `O(n+m)` space and `O(n+m)` time.

---

## 📝 Dry Run

nums1 = [2,3,6], nums2 = [1,4,5,7], k = 4

- Merge -> [1,2,3,4,5,6,7]
    
- 4th element = 4
    

---

## 💻 Code (Java)

```java
// Brute: merge both arrays and pick k-th element (1-based k)
public static int kthElementBrute(int[] nums1, int[] nums2, int k) {
    int n = nums1.length, m = nums2.length;
    int[] merged = new int[n + m];
    int i = 0, j = 0, idx = 0;
    while (i < n && j < m) {
        if (nums1[i] <= nums2[j]) merged[idx++] = nums1[i++];
        else merged[idx++] = nums2[j++];
    }
    while (i < n) merged[idx++] = nums1[i++];
    while (j < m) merged[idx++] = nums2[j++];
    return merged[k - 1]; // k is 1-based
}
```

---

# Better Approach

## 🧠 Intuition (Hinglish)

Space optimize karne ke liye merge-on-the-fly karo using two pointers and stop once you reach k elements. Time `O(k)` and space `O(1)`.

Useful jab k chhota ho compared to n+m.

---

## 📝 Dry Run

nums1 = [2,3,6], nums2 = [1,4,5,7], k = 4

- take 1 (from nums2)
    
- take 2 (from nums1)
    
- take 3 (from nums1)
    
- take 4 (from nums2) → answer 4
    

---

## 💻 Code (Java)

```java
// Better: two-pointer until k elements seen
public static int kthElementTwoPointers(int[] nums1, int[] nums2, int k) {
    int i = 0, j = 0, count = 0;
    int curr = -1;
    while (count < k) {
        if (i < nums1.length && (j >= nums2.length || nums1[i] <= nums2[j])) {
            curr = nums1[i++];
        } else {
            curr = nums2[j++];
        }
        count++;
    }
    return curr;
}
```

---

# Optimal Approach (Binary Search on k)

## 🧠 Intuition (Hinglish)

We can find k-th element in `O(log(min(n,m)))` by doing a binary search on how many elements to take from the first array.

Pick `take` elements from `nums1` (0..min(k, n)). Then take `k - take` from `nums2`. Check if partition valid: lastTakenFrom1 <= firstRemainingFrom2 and lastTakenFrom2 <= firstRemainingFrom1. Use sentinels for boundaries. Adjust `take` accordingly.

This is similar to median-of-two-arrays partition trick but generalized for k.

---

## 📝 Dry Run

nums1=[2,3,6], nums2=[1,4,5,7], k=4

- n=3,m=4
    
- try take=1 from nums1 -> need 3 from nums2
    
    - left1 = nums1[0]=2, right1=nums1[1]=3
        
    - left2 = nums2[2]=5, right2=nums2[3]=7
        
    - check partitions -> adjust take
        
- correct partition yields k-th = 4
    

---

## 💻 Code (Java)

```java
// Optimal: O(log(min(n,m))) time
public static int kthElementBinarySearch(int[] nums1, int[] nums2, int k) {
    int n = nums1.length, m = nums2.length;
    if (n > m) return kthElementBinarySearch(nums2, nums1, k); // ensure nums1 is smaller

    int low = Math.max(0, k - m); // at least k-m must come from nums1
    int high = Math.min(k, n);

    while (low <= high) {
        int take = low + (high - low) / 2;
        int takeFrom2 = k - take;

        int left1 = (take == 0) ? Integer.MIN_VALUE : nums1[take - 1];
        int right1 = (take == n) ? Integer.MAX_VALUE : nums1[take];
        int left2 = (takeFrom2 == 0) ? Integer.MIN_VALUE : nums2[takeFrom2 - 1];
        int right2 = (takeFrom2 == m) ? Integer.MAX_VALUE : nums2[takeFrom2];

        if (left1 <= right2 && left2 <= right1) {
            return Math.max(left1, left2);
        } else if (left1 > right2) {
            high = take - 1;
        } else {
            low = take + 1;
        }
    }

    throw new IllegalArgumentException("k is out of bounds");
}
```

## Complexity

- Time: `O(log(min(n,m)))`
    
- Space: `O(1)`
    

---

# ⚠ Edge Cases / Notes

- `k` is 1-based and `1 <= k <= n+m`.
    
- Arrays may be empty (handle sentinel boundaries).
    
- Be careful with indices when taking 0 or all elements from an array.
    

---

# Examples

- nums1=[2,3,6], nums2=[1,4,5,7], k=4 → 4
    
- nums1=[1,2], nums2=[3,4,5], k=5 → 5