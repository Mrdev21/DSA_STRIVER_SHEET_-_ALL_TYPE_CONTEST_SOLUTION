

# Koko Eating Bananas (Brute Approach)

## 🧠 Intuition (Hinglish)
Brute me hum possible speed `k = 1` se lekar `max(piles)` tak sab try karte hain.

Har speed `k` ke liye:
- calculate total hours needed = sum( ceil(piles[i] / k) )
- agar hours <= H → k valid hai
- smallest k find karna hai (min speed)

Very slow because range huge ho sakta hai (1 to 1e9).

Time : O(max(piles) * n)

---

## 📝 Dry Run
piles = [3, 6, 7, 11], H = 8

Try k = 1 → hours = 3+6+7+11 = 27 > 8  
k = 2 → hours = 2+3+4+6 = 15 > 8  
k = 3 → hours = 1+2+3+4 = 10 > 8  
k = 4 → hours = 1+2+2+3 = 8 <= 8  ✔ candidate  
k = 5 → hours = 1+2+2+3 = 8 <= 8  
... smallest valid = **4**

---

## 💻 Code
```java
public static int kokoBrute(int[] piles, int H) {
    int max = 0;
    for (int p : piles) max = Math.max(max, p);

    int ans = max;
    for (int k = 1; k <= max; k++) {
        if (canEat(piles, H, k)) {
            ans = k;
            break;
        }
    }
    return ans;
}
private static boolean canEat(int[] piles, int H, int k) {
    long hours = 0;
    for (int p : piles) {
        hours += (p + k - 1) / k; // ceil
        if (hours > H) return false;
    }
    return hours <= H;
}
```


# Koko Eating Bananas (Better Approach — Linear Search in Smaller Range)

## 🧠 Intuition (Hinglish)
Better idea:  
Koko ki speed sirf `[1, max(piles)]` ke beech hi hogi.

Hum speed 1 se max tak linear check karte rehte hain, aur jaisi hi ek valid mil jaye, break.

Still worst-case O(max * n) hai, but slightly better.

---

## 📝 Dry Run
Same example → smallest valid = 4, par bruteforce se thoda jaldi mil sakta hai.

---

## 💻 Code
```java
public static int kokoBetter(int[] piles, int H) {
    int max = 0;
    for (int p : piles) max = Math.max(max, p);

    for (int k = 1; k <= max; k++) {
        if (canEat(piles, H, k)) return k;
    }
    return max;
}
```



# Koko Eating Bananas (Optimal Approach — Binary Search on Answer)

## 🧠 Intuition (Hinglish)
Ye **Binary Search on Answer** ka BEST example hai.

Key observation:
- Speed `k` badhti hai to hours **kam hote hain** → monotonic  
- Iska matlab answer sorted fashion follow karta hai → BS possible.

Range:
- low = 1  
- high = max(piles)

Binary search:
- mid = possible eating speed  
- calculate total hours with mid  
- agar hours <= H → speed sahi ya aur slow speed try kar sakte → high = mid - 1  
- else → mid slow hai → low = mid + 1  

At end, low = smallest valid speed.

Time = O(n * log(maxPile))

---

## 📝 Dry Run
piles = [3,6,7,11], H = 8

low=1, high=11  
mid=6 → hours=1+1+2+2=6 ≤8 → valid → high=5  
mid=3 → hours=1+2+3+4=10 >8 → low=4  
mid=4 → hours=1+2+2+3=8 ≤8 → valid → high=3  
STOP → low=4 → **ans = 4**

---

## 💻 Code
```java
public static int kokoOptimal(int[] piles, int H) {
    int max = 0;
    for (int p : piles) max = Math.max(max, p);

    int low = 1, high = max, ans = max;

    while (low <= high) {
        int mid = low + (high - low) / 2;

        if (canEat(piles, H, mid)) {
            ans = mid;        // speed valid
            high = mid - 1;   // try slower speed
        } else {
            low = mid + 1;    // need faster speed
        }
    }
    return ans;
}

// helper: same as above
private static boolean canEat(int[] piles, int H, int k) {
    long hours = 0;
    for (int p : piles) {
        hours += (p + k - 1) / k; // ceil division
        if (hours > H) return false;
    }
    return hours <= H;
}
```


### ⭐ Summary (for revision)

|Approach|Time|Notes|
|---|---|---|
|Brute|O(maxPile * n)|Try all speeds|
|Better|O(maxPile * n)|Early break|
|**Optimal**|**O(n log maxPile)**|**Binary Search on Answer**|