
# 3Sum Problem (Brute Approach)

## 🧠 Intuition (Hinglish)
Brute force me hum simply **teen nested loops** chalakar har possible triplet (i,j,k) try karenge aur check karenge `nums[i] + nums[j] + nums[k] == target` (default target = 0).  
Duplicate triplets ko avoid karne ke liye har valid triplet ko sort karke ek `Set` me daal denge (unique strings/lists).  
Simple hai, lekin O(n^3) time lagta hai.

---

## 📝 Dry Run
Array: `[-1, 0, 1, 2, -1, -4]`, target = 0

Check triplets:
- (-1,0,1) → sum 0 → sorted = [-1,0,1] → add  
- (-1,2,-1) → sum 0 → sorted = [-1,-1,2] → add  
... other combos either repeat above or not sum to 0

Final unique triplets → `[[-1,-1,2], [-1,0,1]]`

---

## 💻 Code
```java
// Brute: O(n^3) with a Set to avoid duplicates
public static List<List<Integer>> threeSumBrute(int[] nums, int target) {
    int n = nums.length;
    Set<List<Integer>> set = new HashSet<>();

    for (int i = 0; i < n; i++) {
        for (int j = i + 1; j < n; j++) {
            for (int k = j + 1; k < n; k++) {
                if (nums[i] + nums[j] + nums[k] == target) {
                    List<Integer> trip = Arrays.asList(nums[i], nums[j], nums[k]);
                    // sort to normalize order for uniqueness
                    List<Integer> tri = new ArrayList<>(trip);
                    Collections.sort(tri);
                    set.add(tri);
                }
            }
        }
    }

    return new ArrayList<>(set);
}
```



# 3Sum Problem (Better Approach)

## 🧠 Intuition (Hinglish)
Better approach me hum ek element fix karenge (`i`) aur baaki ke liye **HashSet** use karke 2-sum style complement search karenge:
- For each `i`, iterate j from i+1..n-1
- Maintain `seen` set of numbers already visited for this `i`
- For current `nums[j]`, complement = target - nums[i] - nums[j]
- Agar complement exists in `seen` → triplet found
Duplicate handling ke liye ek `Set` of lists (sorted) maintain karo.

Time: O(n^2), Space: O(n) (for seen + result-set). Works fine but duplicate control needs care.

---

## 📝 Dry Run
Array: `[-1, 0, 1, 2, -1, -4]`, target = 0

i = 0 (nums[i] = -1):
- j=1 (0): need = 1 → seen {} → add 0 to seen
- j=2 (1): need = 0 → 0 in seen → trip (-1,1,0) → sorted [-1,0,1] add
- j=3 (2): need = -1 → -1 not in seen → add 2 ...
i = 1 (nums[i] = 0): similar, find [-1,0,1] maybe again but set ensures unique
i = 2 ...
Final → `[[-1,-1,2],[-1,0,1]]`

---

## 💻 Code
```java
// Better: fix one element and use HashSet for 2-sum complements (O(n^2))
public static List<List<Integer>> threeSumBetter(int[] nums, int target) {
    int n = nums.length;
    Set<List<Integer>> resultSet = new HashSet<>();

    for (int i = 0; i < n; i++) {
        HashSet<Integer> seen = new HashSet<>();
        for (int j = i + 1; j < n; j++) {
            int need = target - nums[i] - nums[j];
            if (seen.contains(need)) {
                List<Integer> tri = Arrays.asList(nums[i], nums[j], need);
                List<Integer> triSorted = new ArrayList<>(tri);
                Collections.sort(triSorted);
                resultSet.add(triSorted);
            }
            seen.add(nums[j]);
        }
    }

    return new ArrayList<>(resultSet);
}
```



# 3Sum Problem (Optimal Approach)

## 🧠 Intuition (Hinglish)
Standard optimal method: **sort + two-pointer**.  
Steps:
1. Sort array.  
2. Fix index `i` from 0..n-3. For each `i` use two pointers `l = i+1`, `r = n-1` and look for pairs `nums[l] + nums[r] == target - nums[i]`.  
3. If sum too small → l++; too big → r--; if equal → record triplet and move both pointers while skipping duplicates.  
Sorting + two-pointer gives O(n log n + n^2) → O(n^2) time but minimal duplicates handling and simple.

---

## 📝 Dry Run
Array: `[-1, 0, 1, 2, -1, -4]`, target = 0  
Sorted → `[-4, -1, -1, 0, 1, 2]`

i=0 (-4): need 4 → l=1,r=5 → -1+2=1 <4 → l++ ... no triplet  
i=1 (-1): need 1 → l=2 (-1), r=5 (2) → -1+2=1 → match → trip = [-1,-1,2] ; skip duplicates of l/r  
move l,r to next unique → l=4 (1), r=4 stop  
i=2 (-1): skip because same as previous i to avoid duplicates  
i=3 (0): need 0 → l=4,r=5 → 1+2=3 >0 → r-- → l>=r stop

Final → `[[-1,-1,2], [-1,0,1]]`

---

## 💻 Code
```java
// Optimal: sort + two-pointer, with duplicate skipping (classic)
public static List<List<Integer>> threeSumOptimal(int[] nums, int target) {
    List<List<Integer>> res = new ArrayList<>();
    if (nums == null || nums.length < 3) return res;

    Arrays.sort(nums);
    int n = nums.length;

    for (int i = 0; i < n - 2; i++) {
        // skip duplicate starting values
        if (i > 0 && nums[i] == nums[i - 1]) continue;

        int left = i + 1;
        int right = n - 1;
        int need = target - nums[i];

        while (left < right) {
            int sum = nums[left] + nums[right];
            if (sum == need) {
                res.add(Arrays.asList(nums[i], nums[left], nums[right]));

                // move left & right to next different values to avoid duplicates
                int valL = nums[left];
                int valR = nums[right];
                while (left < right && nums[left] == valL) left++;
                while (left < right && nums[right] == valR) right--;
            } else if (sum < need) {
                left++;
            } else {
                right--;
            }
        }
    }

    return res;
}
```



