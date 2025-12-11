

# First & Last Occurrence in Sorted Array (Brute Approach — Linear Scan)

## 🧠 Intuition (Hinglish)
Brute method simple hai:  
- LEFT to RIGHT scan → pehla `arr[i] == x` mile → **first occurrence**  
- RIGHT to LEFT scan → pehla `arr[i] == x` mile → **last occurrence**

Sorted ka koi special use nahi hota brute me.

Time → O(n)

---

## 📝 Dry Run
arr = [1,2,4,4,4,5,7], x = 4  

First:  
i=0→1  
i=1→2  
i=2→4 → first = **2**

Last (reverse):  
i=6→7  
i=5→5  
i=4→4 → last = **4**

---

## 💻 Code
```java
public static int firstOccBrute(int[] arr, int x) {
    for (int i = 0; i < arr.length; i++) {
        if (arr[i] == x) return i;
    }
    return -1;
}

public static int lastOccBrute(int[] arr, int x) {
    for (int i = arr.length - 1; i >= 0; i--) {
        if (arr[i] == x) return i;
    }
    return -1;
}
```



# First & Last Occurrence (Better Approach — Linear + Early Exit)

## 🧠 Intuition (Hinglish)
Sorted array hai →  
- Jaise hi arr[i] > x milta hai, aage values aur bhi bade honge → no need to continue.  
Still O(n) worst-case.

Better only for arrays where x early milta hai.

---

## 📝 Dry Run
arr = [1,3,4,4,4,9,12], x = 4

First:  
i=0→1  
i=1→3  
i=2→4 → return 2

Last: reverse scan:  
i=6→12  
i=5→9  
i=4→4 → return 4

---

## 💻 Code
```java
public static int firstOccBetter(int[] arr, int x) {
    for (int i = 0; i < arr.length; i++) {
        if (arr[i] == x) return i;
        if (arr[i] > x) break;
    }
    return -1;
}

public static int lastOccBetter(int[] arr, int x) {
    for (int i = arr.length - 1; i >= 0; i--) {
        if (arr[i] == x) return i;
        if (arr[i] < x) break; // sorted so earlier ones smaller only
    }
    return -1;
}
```


# First & Last Occurrence (Optimal Approach — Binary Search)

## 🧠 Intuition (Hinglish)
Optimal = **Binary Search**.  
Sorted array me first/last occurrence dhoondhne ka standard trick:

### First Occurrence (leftmost `x`)
- Agar arr[mid] == x → possible answer → left side bhi check karo → `high = mid - 1`
- Agar arr[mid] < x → right me jao  
- Agar arr[mid] > x → left me jao  

### Last Occurrence (rightmost `x`)
- Agar arr[mid] == x → possible → right side me bhi check karo → `low = mid + 1`
- Agar arr[mid] < x → right  
- Agar arr[mid] > x → left  

Time → **O(log n)**  
Space → **O(1)**  
Ye most important interview-pattern hai.

---

## 📝 Dry Run
arr = [1,2,4,4,4,5,7], x = 4

### First occurrence
low=0, high=6  
mid=3 → arr[3]=4 → ans=3 → high=2  
mid=1 → arr[1]=2 <4 → low=2  
mid=2 → arr[2]=4 → ans=2 → high=1 → STOP  
first = **2**

### Last occurrence
low=0, high=6  
mid=3 → arr[3]=4 → ans=3 → low=4  
mid=5 → arr[5]=5>4 → high=4  
mid=4 → arr[4]=4 → ans=4 → low=5 → STOP  
last = **4**

---

## 💻 Code
```java
public static int firstOccOptimal(int[] arr, int x) {
    int low = 0, high = arr.length - 1;
    int ans = -1;

    while (low <= high) {
        int mid = low + (high - low) / 2;

        if (arr[mid] == x) {
            ans = mid;
            high = mid - 1; // go left
        } else if (arr[mid] < x) {
            low = mid + 1;
        } else {
            high = mid - 1;
        }
    }
    return ans;
}

public static int lastOccOptimal(int[] arr, int x) {
    int low = 0, high = arr.length - 1;
    int ans = -1;

    while (low <= high) {
        int mid = low + (high - low) / 2;

        if (arr[mid] == x) {
            ans = mid;
            low = mid + 1; // go right
        } else if (arr[mid] < x) {
            low = mid + 1;
        } else {
            high = mid - 1;
        }
    }
    return ans;
}
```
