
# Binary Search to Find x in Sorted Array (Brute Approach — Linear Scan)

## 🧠 Intuition (Hinglish)
Brute force me hum simply **poora array left se right** scan karenge.  
Jaisi hi element `x` milta hai, index return kar denge.  
Sorted hai ya nahi — koi farak nahi padta.

Time O(n) — but easiest.

---

## 📝 Dry Run
arr = [2, 4, 6, 8, 10], x = 8

i=0 → 2  
i=1 → 4  
i=2 → 6  
i=3 → 8 → found at index **3**

---

## 💻 Code
```java
public static int searchBrute(int[] arr, int x) {
    for (int i = 0; i < arr.length; i++) {
        if (arr[i] == x) return i;
    }
    return -1;
}
```


# Binary Search to Find x in Sorted Array (Better Approach — Early Termination Scan)

## 🧠 Intuition (Hinglish)
Better method sorted property ka half benefit leta hai:  
- Linear scan karte waqt agar kabhi `arr[i] > x` mil gaya → aage ke sab elements aur bhi bade honge (sorted array).  
- So we can break early.

Worst still O(n), but average faster.

---

## 📝 Dry Run
arr = [1,3,5,7,9], x = 6

i=0 → 1  
i=1 → 3  
i=2 → 5  
i=3 → 7 > 6 → break → element is not present  

Result → -1

---

## 💻 Code
```java
public static int searchBetter(int[] arr, int x) {
    for (int i = 0; i < arr.length; i++) {
        if (arr[i] == x) return i;
        if (arr[i] > x) break;
    }
    return -1;
}
```


# Binary Search to Find x in Sorted Array (Optimal Approach)

## 🧠 Intuition (Hinglish)
True optimal = **Binary Search (Divide & Conquer)**.  
Sorted array ka full power use karte hain:

- `mid = (low + high) / 2`  
- agar arr[mid] == x → answer  
- agar arr[mid] < x → element right side me hoga → `low = mid + 1`  
- agar arr[mid] > x → left side me hoga → `high = mid - 1`  

Time O(log n), space O(1).  
Fastest & industry standard.

---

## 📝 Dry Run
arr = [2, 4, 6, 8, 10, 12], x = 10  

low=0, high=5  

mid=2 → arr[2]=6 < 10 → search right (low=3)  
mid=4 → arr[4]=10 → FOUND at **index 4**

---

## 💻 Code
```java
public static int searchOptimal(int[] arr, int x) {
    int low = 0, high = arr.length - 1;

    while (low <= high) {
        int mid = low + (high - low) / 2; // safe mid

        if (arr[mid] == x) return mid;
        else if (arr[mid] < x) low = mid + 1;
        else high = mid - 1;
    }
    return -1;
}
```


