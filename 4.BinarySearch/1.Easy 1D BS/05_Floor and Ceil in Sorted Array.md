
# Floor & Ceil in Sorted Array (Brute Approach — Linear Scan)

## 🧠 Intuition (Hinglish)
Brute force me poora array scan karenge:
- FLOOR → sabse bada element jo x se **chhota ya equal** ho.
- CEIL → sabse chhota element jo x se **bada ya equal** ho.

Sorted hona zaroori nahi brute ke liye.

Time O(n)

---

## 📝 Dry Run
arr = [1, 2, 4, 6, 8, 10], x = 5

Traverse:
1 → ≤5 → floor=1  
2 → ≤5 → floor=2  
4 → ≤5 → floor=4  
6 → ≥5 → ceil=6 (first such)  
8,10 → ignore (ceil found)

Answer → floor=4, ceil=6

---

## 💻 Code
```java
public static int[] floorCeilBrute(int[] arr, int x) {
    int n = arr.length;
    int floor = Integer.MIN_VALUE;
    int ceil = Integer.MAX_VALUE;

    for (int v : arr) {
        if (v <= x) floor = Math.max(floor, v);
        if (v >= x) ceil = Math.min(ceil, v);
    }

    if (floor == Integer.MIN_VALUE) floor = -1;
    if (ceil == Integer.MAX_VALUE) ceil = -1;

    return new int[]{floor, ceil};
}
```


# Floor & Ceil in Sorted Array (Better Approach — Linear + Early Exit)

## 🧠 Intuition (Hinglish)
Sorted array ka fayda:  
- Traverse from left  
- Jaisi hi `arr[i] > x` milta hai → **further elements bhi bade honge** → break.  
- Traverse ke दौरान floor update hota rehega.

Still O(n) worst-case but faster on average.

---

## 📝 Dry Run
arr = [1,3,5,7,9], x = 6

i=0 → 1 ≤6 → floor=1  
i=1 → 3 ≤6 → floor=3  
i=2 → 5 ≤6 → floor=5  
i=3 → 7 >6 → ceil=7 → **break**

Result → floor=5, ceil=7

---

## 💻 Code
```java
public static int[] floorCeilBetter(int[] arr, int x) {
    int floor = -1, ceil = -1;

    for (int v : arr) {
        if (v <= x) floor = v;
        if (v > x) {
            ceil = v;
            break;
        }
    }
    return new int[]{floor, ceil};
}
```


# Floor & Ceil in Sorted Array (Optimal Approach — Binary Search)

## 🧠 Intuition (Hinglish)
Sorted array me best = **Binary Search**.

### FLOOR (greatest ≤ x)
- Agar arr[mid] ≤ x → possible floor → left me bhi dekh sakte ho → low = mid+1  
- Agar arr[mid] > x → right me nahi mil sakta → high = mid-1  

### CEIL (smallest ≥ x)
- Agar arr[mid] ≥ x → possible ceil → left me aur chhota chance → high = mid-1  
- Agar arr[mid] < x → right me hi hoga → low = mid+1  

Dono O(log n) me.

---

## 📝 Dry Run
arr = [2, 4, 6, 8, 10], x = 7

### FLOOR
low=0, high=4  
mid=2 → 6 ≤7 → floor=6, low=3  
mid=3 → 8 >7 → high=2 → STOP  
floor = **6**

### CEIL
low=0, high=4  
mid=2 → 6 <7 → low=3  
mid=3 → 8 ≥7 → ceil=8, high=2 → STOP  
ceil = **8**

---

## 💻 Code
```java
public static int[] floorCeilOptimal(int[] arr, int x) {
    int n = arr.length;
    int floor = -1, ceil = -1;

    // FLOOR (greatest <= x)
    int low = 0, high = n - 1;
    while (low <= high) {
        int mid = low + (high - low) / 2;
        if (arr[mid] <= x) {
            floor = arr[mid];
            low = mid + 1;
        } else {
            high = mid - 1;
        }
    }

    // CEIL (smallest >= x)
    low = 0; high = n - 1;
    while (low <= high) {
        int mid = low + (high - low) / 2;
        if (arr[mid] >= x) {
            ceil = arr[mid];
            high = mid - 1;
        } else {
            low = mid + 1;
        }
    }

    return new int[]{floor, ceil};
}
```
