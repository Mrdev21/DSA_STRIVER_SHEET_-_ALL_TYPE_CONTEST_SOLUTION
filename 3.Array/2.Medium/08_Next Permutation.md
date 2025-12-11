
# Next Permutation (Brute Approach)

## 🧠 Intuition (Hinglish)
Brute approach me hum array ke **saare permutations** generate karenge, unko lexicographic order me sort karenge aur phir current permutation locate karke uske baad wali permutation ko result manaenge.  
Agar current permutation last hai to smallest permutation (sorted ascending) return karenge.

Ye bilkul straightforward hai lekin factorial time aur space leta hai — sirf theory/demo ke liye useful.

---

## 📝 Dry Run
Array: [1, 3, 2]

All permutations (lexicographic sorted):
[1,2,3]
[1,3,2]  ← current
[2,1,3]
[2,3,1]
[3,1,2]
[3,2,1]

Current index = 1 → next = index 2 → [2,1,3]

Result → **[2,1,3]**

---

## 💻 Code
```java
// Brute: generate all permutations, sort them, find next
public static void nextPermutationBrute(int[] arr) {
    List<String> perms = new ArrayList<>();
    permute(arr, 0, perms);

    Collections.sort(perms); // lexicographic order

    String cur = toKey(arr);
    int idx = perms.indexOf(cur);

    String nextKey;
    if (idx == -1 || idx == perms.size() - 1) {
        nextKey = perms.get(0); // wrap to smallest
    } else {
        nextKey = perms.get(idx + 1);
    }

    fromKey(nextKey, arr);
}

private static void permute(int[] a, int l, List<String> out) {
    if (l == a.length) {
        out.add(toKey(a));
        return;
    }
    for (int i = l; i < a.length; i++) {
        swap(a, l, i);
        permute(a, l + 1, out);
        swap(a, l, i);
    }
}

private static String toKey(int[] a) {
    StringBuilder sb = new StringBuilder();
    for (int x : a) {
        sb.append(x).append(','); // delimiter
    }
    return sb.toString();
}

private static void fromKey(String key, int[] a) {
    String[] parts = key.split(",");
    for (int i = 0; i < a.length; i++) {
        a[i] = Integer.parseInt(parts[i]);
    }
}

private static void swap(int[] a, int i, int j) {
    int t = a[i]; a[i] = a[j]; a[j] = t;
}
```



# Next Permutation (Better Approach)

## 🧠 Intuition (Hinglish)
Better approach brute se zyada efficient hai — idea same lexicographic next, lekin hum suffix ko sort karke next find karte hain rather than generating all permutations.

Steps:
1. Right se scan karke pivot index `i` find karo jaha `arr[i] < arr[i+1]`.  
   - Agar aisa koi `i` nahi hai → array descending → sorted largest → return ascending (reverse whole array).
2. In suffix `i+1..end`, find the **smallest element greater than arr[i]** (call index `j`).  
3. Swap arr[i] and arr[j].  
4. Sort the suffix `i+1..end` (ascending) — yeh step ensures next lexicographic smallest.

Time: O(n log n) because of sorting suffix. Space: O(1) inplace (if Arrays.sort used on subarray).

---

## 📝 Dry Run
Array: [1, 3, 2]

1) Find pivot: from right, 3>2? no; 1<3 → pivot i=0 (value 1)  
2) In suffix [3,2], smallest element >1 is 2 at j=2  
3) Swap arr[0] & arr[2] → [2,3,1]  
4) Sort suffix i+1..end (indices 1..2): sort [3,1] → [1,3]  
Result → [2,1,3]

---

## 💻 Code
```java
// Better: pivot + swap with smallest-greater in suffix + sort suffix
public static void nextPermutationBetter(int[] arr) {
    int n = arr.length;
    if (n <= 1) return;

    // 1) find pivot
    int i = n - 2;
    while (i >= 0 && arr[i] >= arr[i + 1]) i--;

    if (i < 0) {
        // highest permutation -> smallest
        reverse(arr, 0, n - 1);
        return;
    }

    // 2) find smallest element > arr[i] in suffix (i+1..n-1)
    int j = i + 1;
    int candidate = -1;
    for (int k = i + 1; k < n; k++) {
        if (arr[k] > arr[i]) {
            if (candidate == -1 || arr[k] < arr[candidate]) {
                candidate = k;
            }
        }
    }

    // 3) swap pivot with candidate
    swap(arr, i, candidate);

    // 4) sort suffix ascending
    Arrays.sort(arr, i + 1, n);
}

private static void reverse(int[] a, int l, int r) {
    while (l < r) { swap(a, l++, r--); }
}
```




# Next Permutation (Optimal Approach)

## 🧠 Intuition (Hinglish)
Classic optimal algorithm — one pass from right plus one reverse. Ye O(n) time, O(1) space solution hai.

Steps (standard):
1. Find largest index `i` such that `arr[i] < arr[i+1]` (scan from right).  
   - Agar koi nahi → array descending → reverse whole array and return.
2. Find largest index `j` (scan from right) such that `arr[j] > arr[i]`.  
   - Note: scanning from right guarantees `j` is the first element greater than pivot and gives the correct successor.
3. Swap `arr[i]` and `arr[j]`.  
4. Reverse suffix `i+1..end` (NOT sort — simple reverse because suffix was in descending order).

Yahi standard Next Permutation algorithm — O(n) and inplace.

---

## 📝 Dry Run
Example A: [1, 2, 3]  
1) i = 1 (arr[1]=2 < arr[2]=3)  
2) j = 2 (arr[2]=3 > 2)  
3) swap → [1,3,2]  
4) reverse suffix (index 2..2) → unchanged → result [1,3,2]

Example B: [3, 2, 1]  
1) no i found (already descending) → reverse whole → [1,2,3]

Example C (duplicates): [1,5,1]  
1) i = 0 (1 < 5)  
2) j from right: arr[2]=1 not>1, arr[1]=5>1 → j=1  
3) swap → [5,1,1]  
4) reverse suffix(1..2) → suffix [1,1] unchanged → result [5,1,1]

---

## 💻 Code
```java
// Optimal: O(n) in-place algorithm
public static void nextPermutationOptimal(int[] arr) {
    int n = arr.length;
    if (n <= 1) return;

    // 1) find pivot i
    int i = n - 2;
    while (i >= 0 && arr[i] >= arr[i + 1]) i--;

    if (i >= 0) {
        // 2) find rightmost successor j
        int j = n - 1;
        while (j > i && arr[j] <= arr[i]) j--;

        // 3) swap pivot and successor
        swap(arr, i, j);
    }

    // 4) reverse suffix (i+1 .. end)
    reverse(arr, i + 1, n - 1);
}

// helper methods (reuse from above)
private static void swap(int[] a, int p, int q) {
    int t = a[p]; a[p] = a[q]; a[q] = t;
}

private static void reverse(int[] a, int l, int r) {
    while (l < r) { swap(a, l++, r--); }
}
```



