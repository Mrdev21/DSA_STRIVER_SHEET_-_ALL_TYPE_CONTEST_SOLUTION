
**Problem (short, Hinglish):** Tumhe existing gas stations ke coordinates diye gaye hain (sorted array `stations[]`). Tum **K naye gas stations** add kar sakte ho. Goal yeh hai ki har do adjacent gas stations ke beech ka **maximum distance** jitna ho sake utna kam ho jaye.

Matlab: K stations add karke hum chahte hain ki **largest gap** minimum possible ban jaaye.

Yeh ek classic "Minimize the maximum" + Binary Search on Answer problem hai.

---

# Brute Approach

## 🧠 Intuition (Hinglish)

Brute idea: har interval me jahan jahan possible ho waha naya station daalte jao, har permutation try karo, har placement ke baad maximum gap calculate karo, fir best choose karo.

But combinations of placements are infinite (continuous coordinates), brute impossible.

Isliye brute mostly theoretical.

---

## 📝 Dry Run

stations = [1, 2, 3, 4, 5, 6, 7, 8, 9], K=3  
Brute me continuous placement options infinite → practical nahi.

---

## 💻 Code (Brute) → Not possible

(Continuous search space. Skip.)

---

# Better Approach

## 🧠 Intuition (Hinglish)

Better idea: Har interval (stations[i+1] - stations[i]) me calculate karo ki kitne stations dalne se gap kitna hoga.

Ek greedy idea: har step pe us interval me station daalo jiska gap sabse bada hai.

Isme priority queue use hoti hai aur har insertion ke baad gap recompute karna padta hai → O(K log n).

Decent hai par binary search se slow.

---

## 📝 Dry Run

stations=[1,10], K=3  
Gap=9  
Insert midway repeatedly → segments=4 → max gap ~2.25

---

## 💻 Code (Better: PQ-based)

```java
import java.util.*;

class Segment {
    double length;
    int count; // how many extra stations added in this segment
    Segment(double length) { this.length = length; this.count = 0; }
    double gap() { return length / (count + 1); }
}

public static double minimizeMaxDistanceBetter(int[] stations, int K) {
    PriorityQueue<Segment> pq = new PriorityQueue<>((a,b) -> Double.compare(b.gap(), a.gap()));

    for (int i = 0; i < stations.length-1; i++) {
        pq.add(new Segment(stations[i+1] - stations[i]));
    }

    while (K-- > 0) {
        Segment s = pq.poll();
        s.count++;
        pq.add(s);
    }

    return pq.peek().gap();
}
```

---

# Optimal Approach (Binary Search on Answer)

## 🧠 Intuition (Hinglish)

Binary Search on Answer.

Assume maximum allowed gap = `mid`.  
Hum check karte hain ki kya hum **at most K** stations add karke sabhi gaps ko `<= mid` bana sakte hain?

Kaise check karein?  
For each interval gap `d = stations[i+1] - stations[i]`:

- Required new stations = `ceil(d / mid) - 1`  
    Total required <= K → feasible.
    

Agar feasible → try smaller gap.  
Agar not feasible → need bigger gap.

---

## 📝 Dry Run

stations = [1,10], K = 3  
low=0, high=9  
Try mid=4.5 → gap=9 → need ceil(9/4.5)-1 = 1 → feasible → high shrinks  
mid=2.25 → need 3 → feasible → high shrinks  
mid=2.0 → need 4 → not feasible → increase low  
Final ~2.25

---

## 💻 Code (Optimal)

```java
public static double minimizeMaxDistanceOptimal(int[] stations, int K) {
    double low = 0, high = stations[stations.length - 1] - stations[0];

    while (high - low > 1e-6) { // precision
        double mid = low + (high - low) / 2;
        if (canAchieve(stations, K, mid)) {
            high = mid;
        } else {
            low = mid;
        }
    }
    return high;
}

private static boolean canAchieve(int[] stations, int K, double dist) {
    int used = 0;
    for (int i = 0; i < stations.length - 1; i++) {
        double gap = stations[i+1] - stations[i];
        used += (int)Math.ceil(gap / dist) - 1;
        if (used > K) return false;
    }
    return true;
}
```

---

# ✅ Complexity Summary

- **Brute:** Impossible (continuous search space)
    
- **Better (PQ):** `O(K log n)`
    
- **Optimal (Binary Search):** `O(n log(range/precision))` → best
    

---

# ⚠ Edge Cases / Notes

- Coordinates sorted hone chahiye
    
- Precision typically `1e-6`
    
- Large gaps me bhi binary search stable
    
- Problem same as LeetCode 774
    

---

# Examples

- stations=[1,2,3,4,5,6], K=2 → max gap reduces accordingly
    
- stations=[1,10], K=3 → ~2.25