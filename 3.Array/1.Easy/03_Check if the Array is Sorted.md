

# Check if the Array is Sorted (Brute Approach)

## 🧠 Intuition (Hinglish)
Brute force me hum array ke har element ko manually baaki sab se compare kar sakte hain.  
Agar koi bhi pair aisa mil jaye jaha arr[j] < arr[i] for j > i,  
matlab array sorted nahi hai.

Ye approach simple comparison-based brute force check hai.

---

## 📝 Dry Run
Array: [1, 2, 3, 5, 4]

Check all later elements:
- 1 → sab thik  
- 2 → sab thik  
- 3 → sab thik  
- 5 → next element = 4 (4 < 5) → ❌ Not Sorted

Result → Array **not sorted**

---

## 💻 Code
```java
public static boolean isSortedBrute(int[] arr) {
    int n = arr.length;
    for (int i = 0; i < n; i++) {
        for (int j = i + 1; j < n; j++) {
            if (arr[j] < arr[i]) {
                return false;
            }
        }
    }
    return true;
}
```




# Check if the Array is Sorted (Better Approach)

## 🧠 Intuition (Hinglish)
Sorted array ka matlab hota hai:  
Har element apne next element se chhota ya equal hona chahiye.  
Toh hume bas adjacent pairs ko check karna hai.  
Ek bhi pair violate ho gaya to array sorted nahi hai.

---

## 📝 Dry Run
Array: [1, 2, 3, 5, 4]

Pairs:
1 ≤ 2 → OK  
2 ≤ 3 → OK  
3 ≤ 5 → OK  
5 ≤ 4 → ❌ → Not Sorted

Result → **Not Sorted**

---

## 💻 Code
```java
public static boolean isSortedBetter(int[] arr) {
    for (int i = 0; i < arr.length - 1; i++) {
        if (arr[i] > arr[i + 1]) return false;
    }
    return true;
}
```



# Check if the Array is Sorted (Optimal Approach)

## 🧠 Intuition (Hinglish)
Optimal approach me hum previous element ko track karenge.  
Agar kabhi current element previous se chhota mil gaya → array sorted nahi hai.  
Yeh same better approach ka clean aur readable version hota hai (for-each use kar sakte ho).

---

## 📝 Dry Run
Array: [1, 2, 3, 5, 4]

prev = 1  
curr = 2 → OK  
prev = 2  
curr = 3 → OK  
prev = 3  
curr = 5 → OK  
prev = 5  
curr = 4 → ❌ → Not Sorted

Result → **Not Sorted**

---

## 💻 Code
```java
public static boolean isSortedOptimal(int[] arr) {
    int prev = arr[0];
    for (int i = 1; i < arr.length; i++) {
        if (arr[i] < prev) return false;
        prev = arr[i];
    }
    return true;
}
```
