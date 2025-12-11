

# Implement Upper Bound (Brute Approach — Linear Scan)

## 🧠 Intuition (Hinglish)
Upper bound ka matlab: **pehla index i jaha arr[i] > x**.  
Brute force me hum simple left→right scan karenge, jaisi hi koi element `> x` mile return kar denge.  
Sorted hona zaroori nahi — lekin agar sorted hai to better/optimal use karna chahiye.

Time O(n), space O(1).

---

## 📝 Dry Run
arr = [1, 2, 4, 4, 5], x = 4

i=0 → 1 <= 4  
i=1 → 2 <= 4  
i=2 → 4 <= 4  
i=3 → 4 <= 4  
i=4 → 5 > 4 → **return 4**

Upper bound index = **4**

---

## 💻 Code
```java
public static int upperBoundBrute(int[] arr, int x) {
    for (int i = 0; i < arr.length; i++) {
        if (arr[i] > x) return i;
    }
    return arr.length; // not found -> return n
}
```


# Implement Upper Bound (Better Approach — Linear Scan with Early Exit on Sorted Array)

## 🧠 Intuition (Hinglish)
Agar array **sorted** hai to linear scan me early exit ka fayda milta hai:  
Jab arr[i] > x mil jaye turant return kar do — lekin worst-case phir bhi O(n) hi rahega.  
Ye approach chhote arrays ya mostly-early-found cases ke liye useful hai.

---

## 📝 Dry Run
arr = [1, 3, 5, 7], x = 6

i=0 → 1 <= 6  
i=1 → 3 <= 6  
i=2 → 5 <= 6  
i=3 → 7 > 6 → **return 3**

Upper bound = **3**

---

## 💻 Code
```java
public static int upperBoundBetter(int[] arr, int x) {
    for (int i = 0; i < arr.length; i++) {
        if (arr[i] > x) return i;
    }
    return arr.length;
}
```


# Implement Upper Bound (Optimal Approach — Binary Search)

## 🧠 Intuition (Hinglish)
Sorted array me optimal tarika **binary search** hai.  
Logic (upper_bound style):
- Maintain `low = 0`, `high = n-1`, `ans = n` (default if none found).  
- mid = low + (high-low)/2  
- Agar `arr[mid] > x` → candidate mil gaya → `ans = mid`, phir left side me dekhne ke liye `high = mid - 1`.  
- Agar `arr[mid] <= x` → upper bound right side me hi hogi → `low = mid + 1`.  
Loop end pe `ans` hold karega pehla index with `arr[i] > x` (or n).

Time O(log n), space O(1). Ye C++ `upper_bound` jaisa behaviour deta hai.

---

## 📝 Dry Run
arr = [1, 2, 4, 4, 5, 9], x = 4

low=0, high=5, ans=6
mid=2 → arr[2]=4 <= 4 → low=3
mid=4 → arr[4]=5 > 4 → ans=4, high=3
mid=3 → arr[3]=4 <=4 → low=4 (loop ends)

return ans = **4**

---

## 💻 Code
```java
public static int upperBoundOptimal(int[] arr, int x) {
    int n = arr.length;
    int low = 0, high = n - 1;
    int ans = n; // default if no element > x

    while (low <= high) {
        int mid = low + (high - low) / 2;
        if (arr[mid] > x) {
            ans = mid;
            high = mid - 1; // try to find earlier index
        } else {
            low = mid + 1;  // arr[mid] <= x -> go right
        }
    }

    return ans;
}
```



