# 4Sum Problem (Brute Approach)

## 🧠 Intuition (Hinglish)
Brute force me hum 4 nested loops chala ke har possible quadruplet (i<j<k<l) try karenge aur check karenge ki sum == target.  
Duplicate quadruplets avoid karne ke liye har valid quad ko sort karke `Set` me daal denge.  
Bahut simple, lekin time O(n^4) — sirf chhote inputs ke liye.

---

## 📝 Dry Run
Array: [1, 0, -1, 0, -2, 2], target = 0

Check combinations:
- (-2, -1, 1, 2) → sum 0 → add
- (-2, 0, 0, 2) → sum 0 → add
- (-1, 0, 0, 1) → sum 0 → add
... aur duplicates skip ho jayenge via set

Result → `[[-2,-1,1,2], [-2,0,0,2], [-1,0,0,1]]` (order arbitrary)

---

## 💻 Code
```java
// Brute: O(n^4) with a Set to avoid duplicate quadruplets
public static List<List<Integer>> fourSumBrute(int[] nums, int target) {
    int n = nums.length;
    Set<List<Integer>> set = new HashSet<>();

    for (int i = 0; i < n; i++) {
        for (int j = i + 1; j < n; j++) {
            for (int k = j + 1; k < n; k++) {
                for (int l = k + 1; l < n; l++) {
                    long sum = 0L + nums[i] + nums[j] + nums[k] + nums[l];
                    if (sum == target) {
                        List<Integer> quad = Arrays.asList(nums[i], nums[j], nums[k], nums[l]);
                        List<Integer> sortedQuad = new ArrayList<>(quad);
                        Collections.sort(sortedQuad);
                        set.add(sortedQuad);
                    }
                }
            }
        }
    }

    return new ArrayList<>(set);
}
```




# 4Sum Problem (Better Approach — Pair-sum HashMap)

## 🧠 Intuition (Hinglish)
Better approach me hum pehle **saare pair sums** store karenge: `sum -> list of pairs (i,j)`.  
Phir har pair-sum `s` ke liye dekhenge agar `target - s` bhi map me hai; dono lists ke pairs combine kar ke quadruplets banao, lekin **indices overlap** na hone chahiye (i,j,k,l distinct).  
Yeh average O(n^2) time to build map, combining phase worst-case heavy but practical for moderate n. Duplicate quads `Set` se remove karlo.

---

## 📝 Dry Run
Array: [1, 0, -1, 0, -2, 2], target = 0

Build pair sums map (examples):
- pairs with sum -2 → [(-2,0) ...]  
- pairs with sum 0 → [(1,-1),(0,0),(-2,2),...]  

Combine sum s and target-s to form quadruplets, ensure indices distinct, sort quadruplet and add to set.

Found → `[-2,-1,1,2]`, `[-2,0,0,2]`, `[-1,0,0,1]`.

---

## 💻 Code
```java
// Better: pair-sum hashmap approach (careful index check + dedupe)
public static List<List<Integer>> fourSumBetter(int[] nums, int target) {
    int n = nums.length;
    Map<Integer, List<int[]>> map = new HashMap<>(); // sum -> list of pairs (i,j)

    // build map of all pair sums
    for (int i = 0; i < n; i++) {
        for (int j = i + 1; j < n; j++) {
            int sum = nums[i] + nums[j];
            map.computeIfAbsent(sum, k -> new ArrayList<>()).add(new int[]{i, j});
        }
    }

    Set<List<Integer>> resultSet = new HashSet<>();

    // for each sum s, try to find target-s
    for (Map.Entry<Integer, List<int[]>> e : map.entrySet()) {
        int s = e.getKey();
        int need = target - s;
        if (!map.containsKey(need)) continue;

        List<int[]> list1 = e.getValue();
        List<int[]> list2 = map.get(need);

        for (int[] p1 : list1) {
            for (int[] p2 : list2) {
                int i = p1[0], j = p1[1];
                int k = p2[0], l = p2[1];

                // ensure indices are all distinct
                if (i == k || i == l || j == k || j == l) continue;

                List<Integer> quad = Arrays.asList(nums[i], nums[j], nums[k], nums[l]);
                List<Integer> sortedQuad = new ArrayList<>(quad);
                Collections.sort(sortedQuad);
                resultSet.add(sortedQuad);
            }
        }
    }

    return new ArrayList<>(resultSet);
}
```


# 4Sum Problem (Optimal Approach — Sort + Two-loops + Two-pointers)

## 🧠 Intuition (Hinglish)
Standard interview-optimal approach: **sort the array**, fir fix do indices `i` aur `j` (first two numbers), aur remaining do ko *two-pointer* se find karo (`left = j+1`, `right = n-1`).  
Sorting helps easily skip duplicates and two-pointer finds pairs in O(n) for each (i,j).  
Total time O(n^3) (best practical), space O(1) extra (besides result). Yeh LC-style accepted solution.

Key: skip duplicates for `i`, `j`, `left` and `right` to avoid repeated quads.

---

## 📝 Dry Run
Array: [1, 0, -1, 0, -2, 2], target = 0  
Sorted → [-2, -1, 0, 0, 1, 2]

- i=0 (-2), j=1 (-1): need = 3 → left=2,right=5 → 0+2=2<3 → left++ ... find (-2,-1,1,2)
- i=0 (-2), j=2 (0): need = 2 → left=3,right=5 → 0+2=2 → (-2,0,0,2)
- i=1 (-1), j=2 (0): need = 1 → left=3,right=5 → 0+2=2>1 → right-- ... find (-1,0,0,1)
Duplicates skipped via while-loops

Final → `[[-2,-1,1,2], [-2,0,0,2], [-1,0,0,1]]`

---

## 💻 Code
```java
// Optimal: sort + two nested loops + two-pointer, skip duplicates
public static List<List<Integer>> fourSumOptimal(int[] nums, int target) {
    List<List<Integer>> res = new ArrayList<>();
    if (nums == null || nums.length < 4) return res;

    Arrays.sort(nums);
    int n = nums.length;

    for (int i = 0; i < n - 3; i++) {
        // skip duplicates for i
        if (i > 0 && nums[i] == nums[i - 1]) continue;

        for (int j = i + 1; j < n - 2; j++) {
            // skip duplicates for j
            if (j > i + 1 && nums[j] == nums[j - 1]) continue;

            int left = j + 1;
            int right = n - 1;
            long need = (long)target - nums[i] - nums[j];

            while (left < right) {
                long twoSum = (long)nums[left] + nums[right];
                if (twoSum == need) {
                    res.add(Arrays.asList(nums[i], nums[j], nums[left], nums[right]));

                    int lv = nums[left], rv = nums[right];
                    // skip duplicates for left and right
                    while (left < right && nums[left] == lv) left++;
                    while (left < right && nums[right] == rv) right--;
                } else if (twoSum < need) {
                    left++;
                } else {
                    right--;
                }
            }
        }
    }

    return res;
}
```



