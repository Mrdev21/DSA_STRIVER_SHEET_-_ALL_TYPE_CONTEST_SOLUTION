
 # Merge Overlapping Subintervals (Brute Approach)

## 🧠 Intuition (Hinglish)
Brute force me hum har interval ko uthake baaki intervals se compare karenge aur agar overlap ho to unko merge kar denge.  
Iska straightforward way: repeatedly scan list, jab koi pair overlap mile to unko merge karke list ko update karo, jab tak koi aur merge na ho.  
Simple hai par worst-case me bahut baar list update karni padti hai → O(n^2).

---

## 📝 Dry Run
Intervals: [[1,4], [2,5], [7,9], [8,10]]

Start list:
- check [1,4] with [2,5] → overlap (since 2 <= 4) → merge → [1,5]
List becomes: [[1,5],[7,9],[8,10]]

- restart scanning (or continue): [1,5] vs [7,9] → no overlap  
- [7,9] vs [8,10] → overlap → merge → [7,10]
List: [[1,5],[7,10]]

No more merges → final: [[1,5],[7,10]]

---

## 💻 Code
```java
// Brute: repeatedly try to merge any overlapping pair until no change.
// Time: worst O(n^2) (because each merge may cause rescans); Space: O(n)
public static List<int[]> mergeIntervalsBrute(List<int[]> intervals) {
    if (intervals == null || intervals.size() <= 1) return intervals == null ? new ArrayList<>() : new ArrayList<>(intervals);

    boolean changed = true;
    List<int[]> list = new ArrayList<>(intervals);

    while (changed) {
        changed = false;
        List<int[]> next = new ArrayList<>();
        boolean[] used = new boolean[list.size()];

        for (int i = 0; i < list.size(); i++) {
            if (used[i]) continue;
            int start = list.get(i)[0];
            int end = list.get(i)[1];

            for (int j = i + 1; j < list.size(); j++) {
                if (used[j]) continue;
                int s2 = list.get(j)[0];
                int e2 = list.get(j)[1];
                // check overlap
                if (s2 <= end && e2 >= start) {
                    // merge
                    start = Math.min(start, s2);
                    end = Math.max(end, e2);
                    used[j] = true;
                    changed = true;
                }
            }
            next.add(new int[]{start, end});
        }
        list = next;
    }

    return list;
}
```



# Merge Overlapping Subintervals (Better Approach)

## 🧠 Intuition (Hinglish)
Better approach: **sort by start** pehle.  
Fir ek stack (ya list) me intervals push karte jao:
- current interval ka start agar stack ke top ke end se <= ho → overlap → top ko extend karke merge karo (end = max(top.end, cur.end))
- nahi → push current as new interval

Sort cost O(n log n), merge pass O(n). Overall O(n log n). Space O(n) for result / stack.

---

## 📝 Dry Run
Intervals: [[1,4], [2,5], [7,9], [8,10]]

1) Sort by start → [[1,4],[2,5],[7,9],[8,10]] (already sorted)
2) stack = []
- push [1,4]
- cur=[2,5] → top [1,4] overlaps (2 <= 4) → merge → top becomes [1,5]
- cur=[7,9] → 7 > 5 → push [7,9]
- cur=[8,10] → top [7,9] overlaps (8 <= 9) → merge → top becomes [7,10]
Result stack → [[1,5],[7,10]]

---

## 💻 Code
```java
// Better: sort + stack/list merging
// Time: O(n log n) due to sort, Space: O(n) for result
public static List<int[]> mergeIntervalsBetter(List<int[]> intervals) {
    List<int[]> res = new ArrayList<>();
    if (intervals == null || intervals.size() == 0) return res;

    // sort by start
    intervals.sort((a, b) -> Integer.compare(a[0], b[0]));

    int[] current = intervals.get(0);
    res.add(new int[]{current[0], current[1]});

    for (int i = 1; i < intervals.size(); i++) {
        int[] cur = intervals.get(i);
        int[] last = res.get(res.size() - 1);

        if (cur[0] <= last[1]) {
            // overlap -> merge
            last[1] = Math.max(last[1], cur[1]);
        } else {
            // no overlap -> append
            res.add(new int[]{cur[0], cur[1]});
        }
    }

    return res;
}
```



# Merge Overlapping Subintervals (Optimal Approach)

## 🧠 Intuition (Hinglish)
Optimal approach same as better (sort + single pass) — technically you can't beat the O(n log n) sorting lower bound unless intervals are pre-sorted.  
Main difference: implement merging **in-place** into a single list (no explicit stack), aur handle edge-cases (touching intervals, single-point intervals).  
Edge-case notes: define overlap as `cur.start <= last.end` (treat touching as merge) — agar problem wants strict overlap (`<`) change accordingly.

---

## 📝 Dry Run
Intervals: [[1,3],[3,5],[6,8]]
- after sort: [[1,3],[3,5],[6,8]]
- merge:
  - start with [1,3]
  - [3,5] → 3 <= 3 → merge → [1,5]
  - [6,8] → 6 > 5 → push → final [[1,5],[6,8]]

---

## 💻 Code
```java
// Optimal: sort + in-place merge into output list (same O(n log n) overall).
// Time: O(n log n) (sorting). Space: O(n) for output but O(1) extra besides output.
public static List<int[]> mergeIntervalsOptimal(List<int[]> intervals) {
    List<int[]> res = new ArrayList<>();
    if (intervals == null || intervals.size() == 0) return res;

    // sort by start
    intervals.sort((a, b) -> Integer.compare(a[0], b[0]));

    int curStart = intervals.get(0)[0];
    int curEnd = intervals.get(0)[1];

    for (int i = 1; i < intervals.size(); i++) {
        int s = intervals.get(i)[0];
        int e = intervals.get(i)[1];

        if (s <= curEnd) {
            // overlap -> extend current
            curEnd = Math.max(curEnd, e);
        } else {
            // no overlap -> push current and move to next
            res.add(new int[]{curStart, curEnd});
            curStart = s;
            curEnd = e;
        }
    }
    // push last
    res.add(new int[]{curStart, curEnd});

    return res;
}
```



