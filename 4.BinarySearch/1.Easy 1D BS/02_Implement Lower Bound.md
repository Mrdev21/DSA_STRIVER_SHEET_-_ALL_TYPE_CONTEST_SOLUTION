
# Implement Lower Bound (Brute Approach — Linear Check)

## 🧠 Intuition (Hinglish)
Lower bound ka matlab: **pehla element jo x se chhota nahi ho** (i.e., arr[i] >= x).  
Brute force me hum simply left-to-right scan karenge aur jaisi hi koi value `>= x` milti hai, wahi answer.

Worst-case O(n) time, but simple.

---

## 📝 Dry Run
arr = [2, 3, 5, 7, 9], x = 6

i=0 → 2 < 6  
i=1 → 3 < 6  
i=2 → 5 < 6  
i=3 → 7 >= 6 → **return 3**

Lower bound index = **3**

---

## 💻 Code
```java
public static int lowerBoundBrute(int[] arr, int x) {
    for (int i = 0; i < arr.length; i++) {
        if (arr[i] >= x) return i;
    }
    return arr.length; // not found
}
```

# Implement Lower Bound (Better Approach — Linear + Early Break)

## 🧠 Intuition (Hinglish)
Sorted array ka thoda fayda:  
- Jaise hi arr[i] > x ya arr[i] == x milta hai → aage sab usse bade hi honge.  
- So hum **yahi pe break** kar sakte hain.

Still O(n), but early termination se average-case fast.

---

## 📝 Dry Run
arr = [1, 4, 6, 8], x = 5

i=0 → 1 < 5  
i=1 → 4 < 5  
i=2 → 6 >= 5 → break → **return 2**

Lower bound = **2**

---

## 💻 Code
```java
public static int lowerBoundBetter(int[] arr, int x) {
    for (int i = 0; i < arr.length; i++) {
        if (arr[i] >= x) return i;
    }
    return arr.length;
}
```



# Implement Lower Bound (Optimal Approach — Binary Search)

## 🧠 Intuition (Hinglish)
Optimal = **Binary Search**, because sorted array me first index `arr[i] >= x` dhundhna hai.

Binary search logic:
- Agar arr[mid] >= x → answer ho sakta hai → **right side ke answers need nahi** → go left (high = mid - 1)  
- Agar arr[mid] < x → answer right side me hi hoga → low = mid + 1  
- Best answer track karte jao.

Time: O(log n)  
Space: O(1)  
Ye real lower_bound implementation (like C++ STL).

---

## 📝 Dry Run
arr = [2, 3, 5, 7, 9], x = 6

low=0, high=4  
mid=2 → arr[2]=5 < 6 → left me nahi → low=3  
mid=3 → arr[3]=7 >= 6 → possible ans=3 → go left (high=2)  
loop ends → final ans = **3**

---

## 💻 Code
```java
public static int lowerBoundOptimal(int[] arr, int x) {
    int low = 0, high = arr.length - 1;
    int ans = arr.length; // default if not found

    while (low <= high) {
        int mid = low + (high - low) / 2;

        if (arr[mid] >= x) {
            ans = mid;
            high = mid - 1; // go left
        } else {
            low = mid + 1;  // go right
        }
    }
    return ans;
}
```

