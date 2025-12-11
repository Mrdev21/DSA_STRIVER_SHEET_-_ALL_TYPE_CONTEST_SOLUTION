
# Minimum days to make M bouquets (Brute Approach)

## 🧠 Intuition (Hinglish)
Brute force me hum har possible day `d` (1 se max(bloomDays)) try karenge.  
Har day pe dekhenge kitne bouquets ban sakte hain agar jo flowers `bloomDays[i] <= d` woh hi available hain.  
Agar kisi day pe required `m` bouquets ban gaye → smallest such day return karo.  
Simple, par agar `maxDay` bada hai to slow ho jayega.

**Edge-check:** agar `m * k > n` → impossible → return -1.

---

## 📝 Dry Run
bloomDays = [1, 10, 3, 10, 2], m = 3, k = 1  
n = 5, need 3 bouquets of 1 adjacent flower → possible.

maxDay = 10

Try days:
- day=1 → available indices where day<=1: [0] → bouquets =1 → <3
- day=2 → available [0,4] → bouquets =2 → <3
- day=3 → available [0,2,4] → bouquets =3 → >=3 → STOP → answer **3**

---

## 💻 Code
```java
// Brute: try every day from 1..maxDay
// Time: O(maxDay * n) worst-case, Space: O(1)
public static int minDaysBrute(int[] bloomDays, int m, int k) {
    int n = bloomDays.length;
    if ((long)m * k > n) return -1;

    int maxDay = 0;
    for (int d : bloomDays) if (d > maxDay) maxDay = d;

    for (int day = 1; day <= maxDay; day++) {
        if (canMake(bloomDays, m, k, day)) return day;
    }
    return -1;
}

private static boolean canMake(int[] bloomDays, int m, int k, int day) {
    int bouquets = 0;
    int consec = 0;
    for (int x : bloomDays) {
        if (x <= day) {
            consec++;
            if (consec == k) {
                bouquets++;
                consec = 0;
                if (bouquets >= m) return true;
            }
        } else {
            consec = 0;
        }
    }
    return bouquets >= m;
}
```


# Minimum days to make M bouquets (Better Approach — iterate over unique bloom days)

## 🧠 Intuition (Hinglish)
Har useful candidate day asal me **kisi flower ka bloomDay** hi ho sakta hai (kyunki between two bloom events kuch badlega nahi).  
Isliye brute me 1..maxDay ki jagah sirf **sorted unique bloomDays** par loop karo — average kaafi kam ho jayega.  
Worst-case ye bhi O(n²) ja sakta hai (agar unique days ~ n aur canMake O(n)), par practical me better.

---

## 📝 Dry Run
bloomDays = [1,10,3,10,2], unique sorted = [1,2,3,10]
Try 1 → fail, 2 → fail, 3 → success → answer **3**

---

## 💻 Code
```java
// Better: iterate over sorted unique bloom days
// Time: O(U * n) where U = number of unique days (<= n). Space: O(n) for set/list.
public static int minDaysBetter(int[] bloomDays, int m, int k) {
    int n = bloomDays.length;
    if ((long)m * k > n) return -1;

    // collect unique days
    TreeSet<Integer> set = new TreeSet<>();
    for (int d : bloomDays) set.add(d);

    for (int day : set) {
        if (canMake(bloomDays, m, k, day)) return day;
    }
    return -1;
}

// reuse canMake from brute
```


# Minimum days to make M bouquets (Optimal Approach — Binary Search on days)

## 🧠 Intuition (Hinglish)
Best trick = **binary search on answer space** (days).  
Observation: agar on day `d` tum `m` bouquets bana sakte ho, to for any `d' >= d` bhi bana paoge (monotonic).  
Isliye search range `[1 .. maxDay]` me binary search karke **smallest valid day** dhundo.  
Per check `canMake` O(n) me hota hai, so overall O(n log maxDay). Industry standard.

**Edge-check:** agar `m * k > n` → directly `-1`.

---

## 📝 Dry Run
bloomDays = [1,10,3,10,2], m=3, k=1  
maxDay=10, low=1, high=10

mid=5 → canMake? yes (flowers with day<=5 are indices 0,2,4 → 3 bouquets) → try smaller → high=4, ans=5  
mid=2 → canMake? no → low=3  
mid=3 → canMake? yes → high=2, ans=3  
loop ends → final ans = **3**

---

## 💻 Code
```java
// Optimal: binary search on day range
// Time: O(n * log(maxDay)), Space: O(1)
public static int minDaysBinary(int[] bloomDays, int m, int k) {
    int n = bloomDays.length;
    if ((long)m * k > n) return -1;

    int maxDay = 0;
    for (int d : bloomDays) if (d > maxDay) maxDay = d;

    int low = 1, high = maxDay;
    int ans = -1;
    while (low <= high) {
        int mid = low + (high - low) / 2;
        if (canMake(bloomDays, m, k, mid)) {
            ans = mid;
            high = mid - 1; // try smaller day
        } else {
            low = mid + 1;  // need more days
        }
    }
    return ans;
}

// helper same as before
private static boolean canMake(int[] bloomDays, int m, int k, int day) {
    int bouquets = 0;
    int consec = 0;
    for (int x : bloomDays) {
        if (x <= day) {
            consec++;
            if (consec == k) {
                bouquets++;
                consec = 0;
                if (bouquets >= m) return true;
            }
        } else {
            consec = 0;
        }
    }
    return bouquets >= m;
}
```



