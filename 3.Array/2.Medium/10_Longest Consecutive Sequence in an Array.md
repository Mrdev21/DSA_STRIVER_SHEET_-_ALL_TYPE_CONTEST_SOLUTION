

# Longest Consecutive Sequence in an Array (Brute Approach)

## 🧠 Intuition (Hinglish)
Brute force me hum har element ko uthayenge aur dekhna start karenge ki us element se kitni consecutive values mil rahi hain (num, num+1, num+2 ...).  
Har next value ke liye poore array me linear search karna padega — isliye time O(n^2) ho jayega.  
Simple hai, but slow.

---

## 📝 Dry Run
Array: [100, 4, 200, 1, 3, 2]

Check each element:
- start at 100 → check 101 in array? no → length = 1
- start at 4   → check 5? no → length = 1
- start at 200 → ... → length = 1
- start at 1   → check 2? yes, 3? yes, 4? yes, 5? no → length = 4 (1,2,3,4)
- start at 3   → would find 4 → length smaller or equal
- start at 2   → would find 3,4 → length smaller

Final longest length → **4**

---

## 💻 Code
```java
// Brute: for each element, try to find consecutive numbers by linear search
public static int longestConsecutiveBrute(int[] nums) {
    int n = nums.length;
    if (n == 0) return 0;
    int best = 1;

    for (int i = 0; i < n; i++) {
        int current = nums[i];
        int length = 1;
        while (contains(nums, current + 1)) {
            current++;
            length++;
        }
        best = Math.max(best, length);
    }

    return best;
}

// helper: linear search (O(n))
private static boolean contains(int[] arr, int val) {
    for (int x : arr) {
        if (x == val) return true;
    }
    return false;
}
```



# Longest Consecutive Sequence in an Array (Better Approach — Sort)

## 🧠 Intuition (Hinglish)
Better approach me array ko sort karenge aur phir sorted order me consecutive runs ko scan karenge.  
Sort karne ke baad duplicates ko ignore karna zaroori hai (same number bar-bar aaye to run break nahi hona chahiye).  
Time O(n log n) (sort), space O(1) extra (or O(n) if you clone).

---

## 📝 Dry Run
Array: [100, 4, 200, 1, 3, 2]

After sorting → [1, 2, 3, 4, 100, 200]

Scan:
- 1 → next 2 (consecutive) → length=2  
- 2 → next 3 → length=3  
- 3 → next 4 → length=4  
- 4 → next 100 → break, record max=4  
- continue → isolated numbers 100, 200 → lengths 1

Final longest length → **4**

---

## 💻 Code
```java
// Better: sort then scan (handle duplicates)
public static int longestConsecutiveSort(int[] nums) {
    if (nums == null || nums.length == 0) return 0;
    Arrays.sort(nums);
    int maxLen = 1;
    int currLen = 1;

    for (int i = 1; i < nums.length; i++) {
        if (nums[i] == nums[i - 1]) {
            // skip duplicate
            continue;
        } else if (nums[i] == nums[i - 1] + 1) {
            // consecutive
            currLen++;
        } else {
            // gap
            maxLen = Math.max(maxLen, currLen);
            currLen = 1;
        }
    }

    return Math.max(maxLen, currLen);
}
```


# Longest Consecutive Sequence in an Array (Optimal Approach — HashSet, O(n))

## 🧠 Intuition (Hinglish)
Optimal trick: sab elements ko HashSet me daal do (O(n)).  
Phir har element ke liye sirf usi element ko start samajh kar (agar element-1 set me nahi hai tab) aage badho — num+1, num+2 ... and count karo.  
Isse har element ko overall constant number of times visit karenge → O(n) time, O(n) space.  
Duplicates automatically handled by set.

---

## 📝 Dry Run
Array: [100, 4, 200, 1, 3, 2]

Set = {100,4,200,1,3,2}

Iterate elements:
- 100 → 99 not in set → start run: only 100 → length 1
- 4   → 3 is in set → skip (we only start when x-1 not present)
- 200 → 199 not in set → length 1
- 1   → 0 not in set → extend: 2 in set, 3 in set, 4 in set, 5 not → length 4 (1,2,3,4) → max=4
- 3,2 encountered but skipped as their predecessor exists

Final longest length → **4**

---

## 💻 Code
```java
// Optimal: HashSet one-pass
public static int longestConsecutiveOptimal(int[] nums) {
    if (nums == null || nums.length == 0) return 0;

    HashSet<Integer> set = new HashSet<>();
    for (int x : nums) set.add(x);

    int maxLen = 0;

    for (int num : set) {
        // only start counting if num-1 is not present (num is sequence start)
        if (!set.contains(num - 1)) {
            int curr = num;
            int length = 1;

            while (set.contains(curr + 1)) {
                curr++;
                length++;
            }

            maxLen = Math.max(maxLen, length);
        }
    }

    return maxLen;
}
```




