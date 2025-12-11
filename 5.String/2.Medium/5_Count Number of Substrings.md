
---

**Problem (short, Hinglish):** "Count number of substrings" ek umbrella topic hai — interview me alag-alag variants aate hain: total substrings, distinct substrings, palindromic substrings, substrings with at most / exactly K distinct characters, substrings with sum = K (array / binary string), etc. Yahan main commonly asked variants cover kar raha hoon with Brute → Better → Optimal structure.

---

# 1) Total number of substrings (simple)

## 🧠 Intuition (Hinglish)

String of length `n` ke liye total substrings = `n*(n+1)/2` (every start i and end j with i<=j). No loops required.

---

## 📝 Dry Run

s = "abc" → n=3 → substrings: "a","b","c","ab","bc","abc" → 6 = 3*4/2

---

## 💻 Code (Java)

```java
public static long totalSubstrings(String s) {
    long n = s.length();
    return n * (n + 1) / 2;
}
```

---

# 2) Count palindromic substrings (LeetCode 647)

## 🧠 Intuition (Hinglish)

Brute: check every substring whether palindrome `O(n^3)`. Better: expand-around-center for each center (odd & even) → `O(n^2)`. Optimal: Manacher algorithm `O(n)`.

Expand-around-center is simplest and practically optimal for interviews.

---

## 📝 Dry Run

s = "aaa"  
Centers: i=0 -> expand "a","aa","aaa" etc → total 6 palindromic substrings.

---

## 💻 Code (Java - expand centers)

```java
public static int countPalindromicSubstrings(String s) {
    int n = s.length();
    int count = 0;
    for (int center = 0; center < n; center++) {
        count += expand(s, center, center);     // odd
        count += expand(s, center, center + 1); // even
    }
    return count;
}

private static int expand(String s, int l, int r) {
    int cnt = 0;
    while (l >= 0 && r < s.length() && s.charAt(l) == s.charAt(r)) {
        cnt++; l--; r++;
    }
    return cnt;
}
```

## Complexity

- Time: `O(n^2)` in worst-case (string of same chars)
    
- Space: `O(1)` extra
    

---

# 3) Count substrings with at most K distinct characters (and exactly K using difference)

## 🧠 Intuition (Hinglish)

Sliding-window trick: number of substrings with **at most K** distinct = sum over window expansions of `(right - left + 1)` style counting, but standard approach uses two-pointer counting of substrings ending at `right`. To get **exactly K**, compute `atMost(K) - atMost(K-1)`.

---

## 📝 Dry Run

s = "pqpqs", K=2

- atMost(2) minus atMost(1) → gives exactly 2 distinct substrings count
    

---

## 💻 Code (Java)

```java
// count substrings with at most K distinct
public static long substringsAtMostKDistinct(String s, int K) {
    int n = s.length();
    int[] freq = new int[256];
    int left = 0, distinct = 0;
    long res = 0;
    for (int right = 0; right < n; right++) {
        if (freq[s.charAt(right)]++ == 0) distinct++;
        while (distinct > K) {
            if (--freq[s.charAt(left++)] == 0) distinct--;
        }
        // all substrings ending at right with start in [left..right]
        res += right - left + 1;
    }
    return res;
}

// exactly K = atMost(K) - atMost(K-1)
public static long substringsExactlyKDistinct(String s, int K) {
    if (K == 0) return 0;
    return substringsAtMostKDistinct(s, K) - substringsAtMostKDistinct(s, K - 1);
}
```

## Complexity

- Time: `O(n)` for each atMost call → `O(n)` overall for exactly K (2 passes)
    
- Space: `O(1)` (alphabet 256) or `O(alphabet)`
    

---

# 4) Count substrings with sum = K (array / binary string)

## 🧠 Intuition (Hinglish)

For array of integers (including negatives): use prefix sum + hashmap storing counts of prefix sums seen. For binary string (0/1) variant, same technique works; also sliding window can be used when all numbers non-negative.

Count of subarrays with sum K = for each prefixSum `ps`, add `countMap.get(ps - K)`.

---

## 📝 Dry Run

arr = [1,1,1], K=2  
prefix sums: 1,2,3  
counts: when ps=2 → ps-K=0 (count 1) -> found subarray [0..1]; when ps=3 -> ps-K=1 (count of 1 is 2) -> two subarrays -> total 2

---

## 💻 Code (Java)

```java
public static int countSubarraysWithSumK(int[] arr, int K) {
    Map<Integer,Integer> cnt = new HashMap<>();
    cnt.put(0, 1);
    int ps = 0, res = 0;
    for (int x : arr) {
        ps += x;
        res += cnt.getOrDefault(ps - K, 0);
        cnt.put(ps, cnt.getOrDefault(ps, 0) + 1);
    }
    return res;
}
```

## Complexity

- Time: `O(n)`
    
- Space: `O(n)` for hashmap
    

---

# 5) Count distinct substrings

## 🧠 Intuition (Hinglish)

Distinct substrings count can be computed using suffix array + LCP (`O(n log n)`) or suffix automaton (SAM) (`O(n)`). For interviews, explain suffix array idea or show trie-based approach (O(n^2) space/time) for small n.

SAM gives number of distinct substrings = sum over states `(len[state] - len[link[state]])`.

---

## 📝 Dry Run (small)

s = "ababa"  
Distinct substrings enumerated manually give result (exercise).

---

## 💻 Code (Sketch - Suffix Automaton idea)

```java
// Full SAM implementation is long; here's the formula: build SAM then
// answer = sum_{state != initial} (len[state] - len[link[state]])
```

## Complexity

- Time: `O(n)` for SAM, `O(n log n)` for suffix array
    

---

# ✅ Summary / Tips

- First clarify the exact variant in interview: "count substrings" is ambiguous.
    
- Common practical patterns:
    
    - sliding window → at most K distinct, at most K ones, etc.
        
    - prefix-sum + hashmap → sum == K for arrays (negatives allowed)
        
    - expand-centers / Manacher → palindromic substrings
        
    - suffix automaton / suffix array → distinct substrings
        

---

# Examples

- totalSubstrings("abc") → 6
    
- countPalindromicSubstrings("aaa") → 6
    
- substringsExactlyKDistinct("pqpqs", 2) → 7
    
- countSubarraysWithSumK([1,1,1], 2) → 2
    

---

_File placement suggestion:_ `DSA/Strings/05_Count Number of Substrings.md`