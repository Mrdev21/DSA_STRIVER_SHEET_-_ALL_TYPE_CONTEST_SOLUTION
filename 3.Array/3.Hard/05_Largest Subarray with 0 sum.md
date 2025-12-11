

# Largest Subarray with 0 Sum (Brute Approach)

## 🧠 Intuition (Hinglish)
Brute force me hum har possible subarray try karenge (start i se end j tak) aur uska cumulative sum nikal ke check karenge agar sum == 0 to length calculate karke max update karenge.  
Simple aur direct — time O(n²), space O(1).

---

## 📝 Dry Run
Array: [1, 2, -3, 3, -1, 2]

Check subarrays:
- start 0:
  - j=0 sum=1
  - j=1 sum=3
  - j=2 sum=0 → length = 3 (best so far: start=0,end=2)
  - j=3 sum=3
  - ...
- start 1:
  - j=1 sum=2
  - j=2 sum=-1
  - j=3 sum=2
  - ...
- other starts produce no longer zero-sum subarray

Final → max length = **3**, indices = **(0,2)**

---

## 💻 Code
```java
// Brute: O(n^2) time, O(1) extra space
// Returns {maxLen, startIndex, endIndex}
public static int[] largestZeroSumBrute(int[] arr) {
    int n = arr.length;
    int maxLen = 0;
    int bestL = -1, bestR = -1;

    for (int i = 0; i < n; i++) {
        int sum = 0;
        for (int j = i; j < n; j++) {
            sum += arr[j];
            if (sum == 0) {
                int len = j - i + 1;
                if (len > maxLen) {
                    maxLen = len;
                    bestL = i;
                    bestR = j;
                }
            }
        }
    }

    return new int[]{maxLen, bestL, bestR};
}
```




# Largest Subarray with 0 Sum (Better Approach — Prefix array)

## 🧠 Intuition (Hinglish)
Better variant me hum prefix-sum array bana ke har pair (i,j) ke liye subarray sum O(1) me nikal sakte hain:
`sum(i..j) = prefix[j+1] - prefix[i]`.  
Fir nested loops se O(n^2) me sab pairs check kar lo — sum calculation constant time ho jati hai (thoda faster in practice).

---

## 📝 Dry Run
Array: [1, 2, -3, 3, -1, 2]

prefix (length n+1): [0,1,3,0,3,2,4]

Check pairs i,j:
- i=0,j=2 → prefix[3]-prefix[0]=0 → length=3
- i=3,j=5 → prefix[6]-prefix[3]=4-0=4 ≠0
... final best = length 3 (0..2)

---

## 💻 Code
```java
// Better: prefix-sum + double loop (O(n^2) time, O(n) space)
// Returns {maxLen, startIndex, endIndex}
public static int[] largestZeroSumBetter(int[] arr) {
    int n = arr.length;
    int[] prefix = new int[n + 1];
    prefix[0] = 0;
    for (int i = 0; i < n; i++) prefix[i + 1] = prefix[i] + arr[i];

    int maxLen = 0, bestL = -1, bestR = -1;
    for (int i = 0; i < n; i++) {
        for (int j = i; j < n; j++) {
            if (prefix[j + 1] - prefix[i] == 0) {
                int len = j - i + 1;
                if (len > maxLen) {
                    maxLen = len;
                    bestL = i;
                    bestR = j;
                }
            }
        }
    }

    return new int[]{maxLen, bestL, bestR};
}
```



# Largest Subarray with 0 Sum (Optimal Approach — Prefix-sum + HashMap)

## 🧠 Intuition (Hinglish)
Optimal trick: running prefix sum store karo aur **pehli occurrence** ka index HashMap me rakho.
Agar running prefix same value phir se milta hai to beech wala subarray ka sum 0 hoga.
Isliye jab prefix `p` pehli baar aata hai → map.put(p, index). Jab p repeat ho → candidate length = i - map.get(p).  
Special handle: prefix 0 from start → store map.put(0, -1) se seedha length = i - (-1) = i+1 mil jata hai.
Time O(n), space O(n). Ye standard aur fastest.

---

## 📝 Dry Run
Array: [1, 2, -3, 3, -1, 2]

Iterate with map (map initially {0:-1}):
- i=0: running=1 → map.put(1,0)
- i=1: running=3 → map.put(3,1)
- i=2: running=0 → map contains 0 → length = 2 - (-1) = 3 → best (0..2)
- i=3: running=3 → map contains 3 at index 1 → length = 3 - 1 = 2
- ... final best length = 3 (indices 0..2)

---

## 💻 Code
```java
// Optimal: prefix-sum + first-occurrence HashMap (O(n) time, O(n) space)
// Returns {maxLen, startIndex, endIndex}
public static int[] largestZeroSumOptimal(int[] arr) {
    int n = arr.length;
    Map<Integer, Integer> firstIndex = new HashMap<>();
    firstIndex.put(0, -1); // prefix 0 occurs before array starts

    int running = 0;
    int maxLen = 0;
    int bestL = -1, bestR = -1;

    for (int i = 0; i < n; i++) {
        running += arr[i];

        if (firstIndex.containsKey(running)) {
            int prevIdx = firstIndex.get(running);
            int len = i - prevIdx;
            if (len > maxLen) {
                maxLen = len;
                bestL = prevIdx + 1;
                bestR = i;
            }
        } else {
            firstIndex.put(running, i); // store first occurrence only
        }
    }

    return new int[]{maxLen, bestL, bestR};
}
```



