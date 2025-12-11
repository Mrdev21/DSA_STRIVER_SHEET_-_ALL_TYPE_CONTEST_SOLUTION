
# Search Insert Position (Brute Approach — Linear Scan)

## 🧠 Intuition (Hinglish)
Brute force me hum left se right scan karte hain:  
- Agar arr[i] == x → wahi index return  
- Agar arr[i] > x → x yahi insert hoga → return i  
- Agar last tak nahi mila → x array ke end me jayega → return n  

Time O(n), simple & clear.

---

## 📝 Dry Run
arr = [1, 3, 5, 6], x = 5

i=0 → 1 < 5  
i=1 → 3 < 5  
i=2 → 5 == 5 → **return 2**

---

## 💻 Code
```java
public static int searchInsertBrute(int[] arr, int x) {
    int n = arr.length;
    for (int i = 0; i < n; i++) {
        if (arr[i] >= x) return i;
    }
    return n;
}
```



# Search Insert Position (Better Approach — Linear + Early Exit on Sorted Array)

## 🧠 Intuition (Hinglish)
Sorted array me jaisi hi arr[i] > x mile, x us position pe insert ho sakta hai.  
Best-case early-stop possible, but worst-case O(n).

---

## 📝 Dry Run
arr = [1, 3, 5, 6], x = 2

i=0 → 1 < 2  
i=1 → 3 > 2 → **return 1**

---

## 💻 Code
```java
public static int searchInsertBetter(int[] arr, int x) {
    for (int i = 0; i < arr.length; i++) {
        if (arr[i] >= x) return i;
    }
    return arr.length;
}
```



# Search Insert Position (Optimal Approach — Binary Search)

## 🧠 Intuition (Hinglish)
Optimal approach = **Binary Search**.  
Sorted array me first index find karna hai jaha `arr[i] >= x` ho.  
Ye exactly **lower bound** jaisa behavior hai.

Binary search logic:
- mid nikalo  
- agar arr[mid] >= x → potential answer, left me jao  
- agar arr[mid] < x → right me jao  
- `ans` ko track rakho (default n)

Time: O(log n)  
Space: O(1)

---

## 📝 Dry Run
arr = [1, 3, 5, 6], x = 7

low=0, high=3, ans=4  
mid=1 → 3 < 7 → low=2  
mid=2 → 5 < 7 → low=3  
mid=3 → 6 < 7 → low=4 (loop ends)

Result = **4**

---

## 💻 Code
```java
public static int searchInsertOptimal(int[] arr, int x) {
    int low = 0, high = arr.length - 1;
    int ans = arr.length;

    while (low <= high) {
        int mid = low + (high - low) / 2;

        if (arr[mid] >= x) {
            ans = mid;
            high = mid - 1;
        } else {
            low = mid + 1;
        }
    }
    return ans;
}
```
