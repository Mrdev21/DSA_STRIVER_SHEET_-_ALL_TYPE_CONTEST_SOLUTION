# Longest subarray with sum K (Brute Approach)

## 🧠 Intuition (Hinglish)
Brute force me hum har possible subarray try karenge:  
Start index i se leke end index j tak ka sum nikal ke check karenge agar sum == K to length note karenge.  
Simple aur direct, lekin time O(n^2) lagta hai.

---

## 📝 Dry Run
Array: [1, -1, 5, -2, 3], K = 3

Check all subarrays:
- start 0:
  - [1] = 1
  - [1, -1] = 0
  - [1, -1, 5] = 5
  - [1, -1, 5, -2] = 3 → match → length = 4 (max = 4)
  - [1, -1, 5, -2, 3] = 6
- start 1:
  - [-1] = -1
  - [-1, 5] = 4
  - [-1, 5, -2] = 2
  - [-1, 5, -2, 3] = 5
- start 2:
  - [5] = 5
  - [5, -2] = 3 → match → length = 2 (max still 4)
  - [5, -2, 3] = 6
- start 3:
  - [-2] = -2
  - [-2, 3] = 1
- start 4:
  - [3] = 3 → match → length = 1 (max still 4)

Final longest length → **4**

---

## 💻 Code
```java
public static int longestSubarraySumBrute(int[] arr, int K) {
    int n = arr.length;
    int maxLen = 0;

    for (int i = 0; i < n; i++) {
        int sum = 0;
        for (int j = i; j < n; j++) {
            sum += arr[j];
            if (sum == K) {
                int len = j - i + 1;
                if (len > maxLen) maxLen = len;
            }
        }
    }

    return maxLen;
}
```






# Longest subarray with sum K (Better Approach)

## 🧠 Intuition (Hinglish)
Better approach me hum prefix-sum array bana kar subarray sum ko O(1) me nikal sakte hain:
prefix[i] = sum of elements from 0..i-1 (ya 0..i depending convention).  
Phir nested loops se har pair (i, j) check karenge using prefix sums: sum(i..j) = prefix[j+1] - prefix[i].
Time ab bhi O(n^2) hai lekin sum calculation O(1) ho gaya — code thoda sa faster aur clear ho jata hai.

---

## 📝 Dry Run
Array: [1, -1, 5, -2, 3], K = 3

Compute prefix (prefix[0] = 0):
prefix = [0, 1, 0, 5, 3, 6]  // lengths n+1

Check pairs:
- i=0, j=3 → sum = prefix[4] - prefix[0] = 3 → length = 4 (max = 4)
- i=2, j=3 → sum = prefix[4] - prefix[2] = 3 → length = 2
- i=4, j=4 → sum = prefix[5] - prefix[4] = 3 → length = 1
... other pairs checked similarly

Final longest length → **4**

---

## 💻 Code
```java
public static int longestSubarraySumBetter(int[] arr, int K) {
    int n = arr.length;
    int[] prefix = new int[n + 1];
    prefix[0] = 0;
    for (int i = 0; i < n; i++) {
        prefix[i + 1] = prefix[i] + arr[i];
    }

    int maxLen = 0;
    // check all pairs (i, j) where i <= j
    for (int i = 0; i < n; i++) {
        for (int j = i; j < n; j++) {
            int sum = prefix[j + 1] - prefix[i];
            if (sum == K) {
                int len = j - i + 1;
                if (len > maxLen) maxLen = len;
            }
        }
    }

    return maxLen;
}
```



# Longest subarray with sum K (Optimal Approach)

## 🧠 Intuition (Hinglish)
Optimal approach me hum **prefix-sum + HashMap** use karenge:
- Iterate karte hue running prefix sum (`prefix`).
- Agar `prefix == K` → subarray from 0..i ka sum K → update max length = i+1.
- Agar `(prefix - K)` kisi earlier index pe mila → subarray (index+1 .. i) ka sum K → length = i - index.
- IMPORTANT: longest paane ke liye **har prefix sum ka FIRST occurrence** store karo (earliest index). Isse aage chal ke jo bhi match milega, woh maximum length dega.
Ye O(n) time aur O(n) space solution hai aur negatives ke case me bhi sahi kaam karta hai.

---

## 📝 Dry Run
Array: [1, -1, 5, -2, 3], K = 3

Iterate:
- i=0: prefix=1
  - prefix==K? no
  - prefix-K = -2 not in map
  - store first occurrence map.put(1, 0)
- i=1: prefix=0
  - prefix==K? no
  - prefix-K = -3 not in map
  - store map.put(0,1)
- i=2: prefix=5
  - prefix==K? no
  - prefix-K = 2 not in map
  - store map.put(5,2)
- i=3: prefix=3
  - prefix==K? yes → length = i+1 = 4 → maxLen = 4
  - prefix-K = 0 is in map at index 1 → length = 3 - 1 = 2 (not larger)
  - store map.put(3,3)
- i=4: prefix=6
  - prefix==K? no
  - prefix-K = 3 in map at index 3 → length = 4 - 3 = 1
  - store map.put(6,4)

Final longest length → **4**

---

## 💻 Code
```java
public static int longestSubarraySumOptimal(int[] arr, int K) {
    int n = arr.length;
    HashMap<Integer, Integer> firstIndex = new HashMap<>();
    int prefix = 0;
    int maxLen = 0;

    for (int i = 0; i < n; i++) {
        prefix += arr[i];

        // subarray from 0..i
        if (prefix == K) {
            maxLen = Math.max(maxLen, i + 1);
        }

        // if prefix-K seen before, subarray (index+1 .. i) sums to K
        if (firstIndex.containsKey(prefix - K)) {
            int prevIndex = firstIndex.get(prefix - K);
            maxLen = Math.max(maxLen, i - prevIndex);
        }

        // store earliest index for this prefix sum
        if (!firstIndex.containsKey(prefix)) {
            firstIndex.put(prefix, i);
        }
    }

    return maxLen;
}
```
