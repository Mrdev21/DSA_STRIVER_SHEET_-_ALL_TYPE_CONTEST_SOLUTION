
# Count subarrays with given xor K (Brute Approach)

## 🧠 Intuition (Hinglish)
Brute force me hum har possible subarray (start i se end j) ka XOR nikal kar check karenge agar woh K ke barabar ho to count++.  
Simple hai — par time O(n^2) lagta hai kyunki har start ke liye end traverse karna padta hai (aur XOR compute karna).

---

## 📝 Dry Run
Array = [4, 2, 2, 6, 4], K = 6

Check all subarrays (major steps):
- start=0:
  - [4] → xor=4 → not 6
  - [4,2] → xor = 4 ^ 2 = 6 → count = 1   (subarray 0..1)
  - [4,2,2] → xor = 6 ^ 2 = 4 → not 6
  - [4,2,2,6] → xor = 4 ^ 6 = 2 → not 6
  - [4,2,2,6,4] → xor = 2 ^ 4 = 6 → count = 2  (subarray 0..4)
- start=1:
  - [2] → 2
  - [2,2] → 2 ^ 2 = 0
  - [2,2,6] → 0 ^ 6 = 6 → count = 3  (subarray 1..3)
  - [2,2,6,4] → 6 ^ 4 = 2
- start=2:
  - [2] → 2
  - [2,6] → 2 ^ 6 = 4
  - [2,6,4] → 4 ^ 4 = 0
- start=3:
  - [6] → 6 → count = 4  (subarray 3..3)
  - [6,4] → 6 ^ 4 = 2
- start=4:
  - [4] → 4

Final count = **4**

---

## 💻 Code
```java
public static int countSubarraysXorBrute(int[] arr, int K) {
    int n = arr.length;
    int count = 0;
    for (int i = 0; i < n; i++) {
        int xor = 0;
        for (int j = i; j < n; j++) {
            xor ^= arr[j];
            if (xor == K) count++;
        }
    }
    return count;
}
```



# Count subarrays with given xor K (Better Approach — prefix-xor array + nested loops)

## 🧠 Intuition (Hinglish)
Better (but still O(n^2)) approach me hum prefix-xor array bana lete hain:
`px[0] = 0`, `px[i] = arr[0] ^ arr[1] ^ ... ^ arr[i-1]` (prefix xor up to i-1).  
Subarray xor(i..j) = px[i] ^ px[j+1].  
Phir nested loops se har pair (i,j) check karenge using constant-time xor lookup — thoda faster than recomputing xor for each subarray.

---

## 📝 Dry Run
Array = [4, 2, 2, 6, 4], K = 6

Compute prefix-xor px (length n+1):
px[0] = 0  
px[1] = 0 ^ 4 = 4  
px[2] = 4 ^ 2 = 6  
px[3] = 6 ^ 2 = 4  
px[4] = 4 ^ 6 = 2  
px[5] = 2 ^ 4 = 6

Now subarray xor(i..j) = px[i] ^ px[j+1]:
- (0,1): px[0] ^ px[2] = 0 ^ 6 = 6 → count++
- (0,4): px[0] ^ px[5] = 0 ^ 6 = 6 → count++
- (1,3): px[1] ^ px[4] = 4 ^ 2 = 6 → count++
- (3,3): px[3] ^ px[4] = 4 ^ 2 = 6 → count++
Total = **4**

---

## 💻 Code
```java
public static int countSubarraysXorBetter(int[] arr, int K) {
    int n = arr.length;
    int[] px = new int[n + 1];
    px[0] = 0;
    for (int i = 0; i < n; i++) px[i + 1] = px[i] ^ arr[i];

    int count = 0;
    for (int i = 0; i < n; i++) {
        for (int j = i; j < n; j++) {
            int subXor = px[i] ^ px[j + 1];
            if (subXor == K) count++;
        }
    }
    return count;
}
```




# Count subarrays with given xor K (Optimal Approach — Prefix-xor + HashMap, O(n))

## 🧠 Intuition (Hinglish)
Optimal trick: while iterating array maintain running prefix-xor `px`.  
We need number of earlier indices `i` such that `px ^ px_i == K` → equivalently `px_i == px ^ K`.  
So maintain `freq` map of previous prefix-xor frequencies.  
- Initialize `freq.put(0,1)` because prefix-xor 0 exists before array starts.  
- For each index, update `px ^= arr[i]`.  
- Add `freq.getOrDefault(px ^ K, 0)` to answer (these are counts of prefixes that make subarray xor = K).  
- Then increment `freq[px]`.  
This is O(n) time and O(n) space and works for all integers.

---

## 📝 Dry Run
Array = [4, 2, 2, 6, 4], K = 6

Start: freq = {0:1}, px = 0, count = 0

i=0 (4):
 px = 0 ^ 4 = 4
 need = px ^ K = 4 ^ 6 = 2 → freq[2] = 0 → count += 0
 freq.put(4,1) → freq = {0:1,4:1}

i=1 (2):
 px = 4 ^ 2 = 6
 need = 6 ^ 6 = 0 → freq[0] = 1 → count += 1  (subarray 0..1)
 freq.put(6,1) → freq = {0:1,4:1,6:1}

i=2 (2):
 px = 6 ^ 2 = 4
 need = 4 ^ 6 = 2 → freq[2] = 0 → count += 0
 freq.put(4,2) → freq = {0:1,4:2,6:1}

i=3 (6):
 px = 4 ^ 6 = 2
 need = 2 ^ 6 = 4 → freq[4] = 2 → count += 2  (subarrays 0..4 and 1..3)
 freq.put(2,1) → freq = {0:1,4:2,6:1,2:1}
 (count now 3)

i=4 (4):
 px = 2 ^ 4 = 6
 need = 6 ^ 6 = 0 → freq[0] = 1 → count += 1  (subarray 3..3)
 freq.put(6,2) → freq = {0:1,4:2,6:2,2:1}
 Final count = **4**

---

## 💻 Code
```java
public static int countSubarraysXorOptimal(int[] arr, int K) {
    int count = 0;
    int px = 0;
    HashMap<Integer, Integer> freq = new HashMap<>();
    freq.put(0, 1); // empty prefix

    for (int x : arr) {
        px ^= x;
        int need = px ^ K;
        count += freq.getOrDefault(need, 0);
        freq.put(px, freq.getOrDefault(px, 0) + 1);
    }

    return count;
}
```



