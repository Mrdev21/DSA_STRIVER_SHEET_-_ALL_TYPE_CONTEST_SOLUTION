

# Brute Approach

## 🧠 Intuition (Hinglish)

Brute me hum saare possible ways try karte hain jisme array ko **exactly k subarrays** me split kiya jaaye. Har split ke liye:

- Har subarray ka sum nikalo
    
- Unka **maximum sum** nikalo
    
- Jo split ka maximum sum minimum ho, wahi answer
    

Par number of partitions → exponential. Practically impossible.

---

## 📝 Dry Run

nums = [7,2,5,10,8], k = 2  
Possible splits:

- | 7 | 2,5,10,8 → max = 25
    
- | 7,2 | 5,10,8 → max = 23
    
- | 7,2,5 | 10,8 → max = 18
    
- | 7,2,5,10 | 8 → max = 24
    
- | 7,2,5 | 10,8 → best = 18
    

Brute confirms correctness but too slow.

---

## 💻 Code

```java
// Exponential recursion: NOT practical, only conceptual brute
public static int splitArrayBrute(int[] nums, int k) {
    return solve(nums, 0, k);
}

private static int solve(int[] nums, int idx, int k) {
    if (k == 1) return sum(nums, idx, nums.length - 1);
    int best = Integer.MAX_VALUE;
    for (int end = idx; end < nums.length - (k - 1); end++) {
        int left = sum(nums, idx, end);
        int right = solve(nums, end + 1, k - 1);
        best = Math.min(best, Math.max(left, right));
    }
    return best;
}

private static int sum(int[] nums, int l, int r) {
    int s = 0;
    for (int i = l; i <= r; i++) s += nums[i];
    return s;
}
```

---

# Better Approach

## 🧠 Intuition (Hinglish)

Brute se better: max subarray sum ka range known hota hai:

- Lower bound = `max(nums)` (largest element must fit in some subarray)
    
- Upper bound = `sum(nums)` (saara array ek subarray)
    

Ab hum sequentially try karein from `max(nums)` to `sum(nums)`, aur check karein ki is capacity se array ko `k` subarrays me split kiya jaa sakta hai.

Still linear scan over large range → slow.

---

## 📝 Dry Run

nums = [7,2,5,10,8], k = 2  
max = 10, sum = 32  
Try cap 10..32  
Eventually cap = 18 works → answer 18

---

## 💻 Code

```java
public static int splitArrayBetter(int[] nums, int k) {
    long sum = 0;
    int mx = 0;
    for (int n : nums) { sum += n; mx = Math.max(mx, n); }

    for (long cap = mx; cap <= sum; cap++) {
        if (subarraysNeeded(nums, cap) <= k) return (int) cap;
    }
    return (int) sum;
}

private static int subarraysNeeded(int[] nums, long cap) {
    long curr = 0;
    int cnt = 1;
    for (int n : nums) {
        if (n > cap) return Integer.MAX_VALUE;
        if (curr + n <= cap) curr += n;
        else {
            cnt++;
            curr = n;
        }
    }
    return cnt;
}
```

---

# Optimal Approach (Binary Search on Answer)

## 🧠 Intuition (Hinglish)

This is **Binary Search on the Answer** problem.

Key fact:  
Agar `cap` max subarray sum ke andar hum array ko `k` subarrays me split kar sakte hain → har `cap' >= cap` bhi valid hoga.

Range monotonic hota hai → binary search.

Range:

- low = `max(nums)`
    
- high = `sum(nums)`
    

Greedy feasibility check: left→right accumulate jab tak cap exceed na ho, fir new subarray.

---

## 📝 Dry Run

nums = [7,2,5,10,8], k = 2  
max = 10, sum = 32  
low=10, high=32

mid=21 → needed=2 → valid → ans=21, high=20  
mid=15 → needed=3 → invalid → low=16  
mid=18 → needed=2 → valid → ans=18, high=17  
mid=16 → needed=3 → invalid → low=17  
mid=17 → needed=3 → invalid → low=18  
stop → answer = 18

---

## 💻 Code

```java
public static int splitArrayOptimal(int[] nums, int k) {
    long sum = 0;
    int mx = 0;
    for (int n : nums) { sum += n; mx = Math.max(mx, n); }

    long low = mx, high = sum, ans = sum;

    while (low <= high) {
        long mid = low + (high - low) / 2;
        if (subarraysNeeded(nums, mid) <= k) {
            ans = mid;
            high = mid - 1;
        } else {
            low = mid + 1;
        }
    }
    return (int) ans;
}
```

---

# ✅ Complexity Summary

- **Brute:** Exponential → useless
    
- **Better:** `O((sum-max)*n)` → big range = slow
    
- **Optimal:** `O(n log(sum-max))` → best approach
    

---

# ⚠ Edge Cases / Notes

- If `k == 1` → answer = sum(nums)
    
- If `k == nums.length` → answer = max(nums)
    
- Use `long` for sums
    
- Feasibility greedy left→right must be used
    

---

# Examples

- nums=[7,2,5,10,8], k=2 → answer=18
    
- nums=[1,4,4], k=3 → answer=4