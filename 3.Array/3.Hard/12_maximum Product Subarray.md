
# Reverse Pairs — Brute Approach (Approach: Pairwise check)

## 🧠 Intuition (Hinglish)
Brute me hum har possible pair `(i, j)` (i < j) check karenge aur dekhenge kya `nums[i] > 2 * nums[j]`.  
Simple hai, direct, lekin time O(n²) — small arrays ke liye theek.

---

## 📝 Dry Run
nums = [1, 3, 2, 3, 1]

Check pairs:
- i=0 (1): compare with j>0 → 1 > 2*3? no; 1>2*2? no; 1>2*3? no; 1>2*1? no
- i=1 (3): j=2 → 3 > 2*2? 3>4? no; j=3 → 3>2*3? no; j=4 → 3>2*1? 3>2 yes → count=1 (pair (1,4))
- i=2 (2): j=3 → 2>2*3? no; j=4 → 2>2*1? 2>2? no (strict >)
- i=3 (3): j=4 → 3>2*1? 3>2 yes → count=2 (pair (3,4))

Final count = **2**

---

## 💻 Code
```java
// Brute: O(n^2)
public static long reversePairsBrute(int[] nums) {
    int n = nums.length;
    long count = 0;
    for (int i = 0; i < n; i++) {
        for (int j = i + 1; j < n; j++) {
            if ((long)nums[i] > 2L * (long)nums[j]) count++;
        }
    }
    return count;
}
```



# Reverse Pairs — Better Approach (Approach: Binary Indexed Tree / Coordinate Compression)

## 🧠 Intuition (Hinglish)
Better approach me hum coordinate-compression + Fenwick Tree (BIT) use kar sakte hain:
- Idea: iterate `j` from left → for current `nums[j]` count how many previous `nums[i]` (i < j) satisfy `nums[i] > 2*nums[j]`.
- Agar hum compress kar lein sari values aur unke doubles (`nums` and `2*nums`), to BIT se prefix sums puch kar `count = seenSoFar - sum(indexOf(2*nums[j]))` mil jayega (seenSoFar = number of elements already added).
- Update: jab ek element `nums[j]` ko "seen" karenge to BIT me `indexOf(nums[j])++`.
- Time O(n log n), Space O(n).

**Important:** use `long` when computing `2*nums[j]`.

---

## 📝 Dry Run
nums = [1, 3, 2, 3, 1]

Build values to compress: {1,3,2,3,1} U {2*1,2*3,2*2,2*3,2*1} = {1,2,3,6,4} unique sorted: [1,2,3,4,6]
Map: 1→1, 2→2, 3→3, 4→4, 6→5 (1-based for BIT)

Iterate left→right, keep BIT of seen numbers (counts):
- j=0 num=1: need indexOf(2*1)=2 → seenSoFar=0 → add 0; update index(1)
- j=1 num=3: need idx(6)=5 → seenSoFar=1 → sum(idx(6))=1 → greater = seenSoFar - sum(5) =0 → update idx(3)
- j=2 num=2: need idx(4)=4 → seenSoFar=2 → sum(4)=? counts <=4 = counts of 1 and 3? =2 → greater=0 → update idx(2)
- j=3 num=3: need idx(6)=5 → seenSoFar=3 → sum(5)=3 → greater=0 → update idx(3)
- j=4 num=1: need idx(2)=2 → seenSoFar=4 → sum(2)= counts <=2 = counts of 1 and 2 = 2 → greater = 4-2 = 2 → add 2 (pairs (i=1,4) and (i=3,4) as found earlier)

Total = **2**

---

## 💻 Code
```java
// Better: BIT + coordinate compression. O(n log n)
public static class Fenwick {
    int n;
    int[] bit;
    public Fenwick(int n) {
        this.n = n;
        this.bit = new int[n + 1];
    }
    public void add(int idx, int delta) {
        for (; idx <= n; idx += idx & -idx) bit[idx] += delta;
    }
    // sum of 1..idx
    public int sum(int idx) {
        int s = 0;
        for (; idx > 0; idx -= idx & -idx) s += bit[idx];
        return s;
    }
}

public static long reversePairsBIT(int[] nums) {
    int n = nums.length;
    // collect values and doubles
    TreeSet<Long> set = new TreeSet<>();
    for (int x : nums) {
        set.add((long)x);
        set.add(2L * (long)x);
    }

    // coordinate compression map value -> index (1-based)
    Map<Long, Integer> idx = new HashMap<>();
    int i = 0;
    for (Long v : set) idx.put(v, ++i);

    Fenwick fw = new Fenwick(idx.size());
    long count = 0;
    int seen = 0;

    for (int x : nums) {
        long twoX = 2L * (long)x;
        Integer pos = idx.get(twoX);
        // number of seen elements greater than 2*x = seen - count <= pos
        int leq = (pos == null) ? fw.sum(idx.size()) : fw.sum(pos);
        count += (seen - leq);
        // now mark current number as seen
        fw.add(idx.get((long)x), 1);
        seen++;
    }

    return count;
}
```




# Reverse Pairs — Better Approach (Approach: Binary Indexed Tree / Coordinate Compression)

## 🧠 Intuition (Hinglish)
Better approach me hum coordinate-compression + Fenwick Tree (BIT) use kar sakte hain:
- Idea: iterate `j` from left → for current `nums[j]` count how many previous `nums[i]` (i < j) satisfy `nums[i] > 2*nums[j]`.
- Agar hum compress kar lein sari values aur unke doubles (`nums` and `2*nums`), to BIT se prefix sums puch kar `count = seenSoFar - sum(indexOf(2*nums[j]))` mil jayega (seenSoFar = number of elements already added).
- Update: jab ek element `nums[j]` ko "seen" karenge to BIT me `indexOf(nums[j])++`.
- Time O(n log n), Space O(n).

**Important:** use `long` when computing `2*nums[j]`.

---

## 📝 Dry Run
nums = [1, 3, 2, 3, 1]

Build values to compress: {1,3,2,3,1} U {2*1,2*3,2*2,2*3,2*1} = {1,2,3,6,4} unique sorted: [1,2,3,4,6]
Map: 1→1, 2→2, 3→3, 4→4, 6→5 (1-based for BIT)

Iterate left→right, keep BIT of seen numbers (counts):
- j=0 num=1: need indexOf(2*1)=2 → seenSoFar=0 → add 0; update index(1)
- j=1 num=3: need idx(6)=5 → seenSoFar=1 → sum(idx(6))=1 → greater = seenSoFar - sum(5) =0 → update idx(3)
- j=2 num=2: need idx(4)=4 → seenSoFar=2 → sum(4)=? counts <=4 = counts of 1 and 3? =2 → greater=0 → update idx(2)
- j=3 num=3: need idx(6)=5 → seenSoFar=3 → sum(5)=3 → greater=0 → update idx(3)
- j=4 num=1: need idx(2)=2 → seenSoFar=4 → sum(2)= counts <=2 = counts of 1 and 2 = 2 → greater = 4-2 = 2 → add 2 (pairs (i=1,4) and (i=3,4) as found earlier)

Total = **2**

---

## 💻 Code
```java
// Better: BIT + coordinate compression. O(n log n)
public static class Fenwick {
    int n;
    int[] bit;
    public Fenwick(int n) {
        this.n = n;
        this.bit = new int[n + 1];
    }
    public void add(int idx, int delta) {
        for (; idx <= n; idx += idx & -idx) bit[idx] += delta;
    }
    // sum of 1..idx
    public int sum(int idx) {
        int s = 0;
        for (; idx > 0; idx -= idx & -idx) s += bit[idx];
        return s;
    }
}

public static long reversePairsBIT(int[] nums) {
    int n = nums.length;
    // collect values and doubles
    TreeSet<Long> set = new TreeSet<>();
    for (int x : nums) {
        set.add((long)x);
        set.add(2L * (long)x);
    }

    // coordinate compression map value -> index (1-based)
    Map<Long, Integer> idx = new HashMap<>();
    int i = 0;
    for (Long v : set) idx.put(v, ++i);

    Fenwick fw = new Fenwick(idx.size());
    long count = 0;
    int seen = 0;

    for (int x : nums) {
        long twoX = 2L * (long)x;
        Integer pos = idx.get(twoX);
        // number of seen elements greater than 2*x = seen - count <= pos
        int leq = (pos == null) ? fw.sum(idx.size()) : fw.sum(pos);
        count += (seen - leq);
        // now mark current number as seen
        fw.add(idx.get((long)x), 1);
        seen++;
    }

    return count;
}
```


