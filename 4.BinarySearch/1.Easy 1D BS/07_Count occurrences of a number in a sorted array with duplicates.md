


# Count Occurrences of x in Sorted Array (Brute Approach — Linear Scan)

## 🧠 Intuition (Hinglish)
Brute approach me simply pura array scan karenge aur jitne bhi elements `x` ke equal honge unka counter badhate jayenge.  
Sorted hone ka koi fayda nahi.

Time: O(n)

---

## 📝 Dry Run
arr = [1,2,4,4,4,5,7], x = 4

Traverse:
1,2 skip  
4 → count=1  
4 → count=2  
4 → count=3  
5,7 skip

Final count = **3**

---

## 💻 Code
```java
public static int countOccBrute(int[] arr, int x) {
    int count = 0;
    for (int v : arr) {
        if (v == x) count++;
    }
    return count;
}
```
# Count Occurrences (Better Approach — Find first match then expand)

## 🧠 Intuition (Hinglish)
Sorted array me ek trick:  
- Pehle linear scan se first `x` dhoondo.  
- Fir uske aage right side me tab tak jao jab tak `x` milta rahe.  

Worst-case O(n), par best-case faster.

---

## 📝 Dry Run
arr = [1,2,4,4,4,5], x = 4

First 4 at index 2  
Expand:
i=2 → 4  
i=3 → 4  
i=4 → 4  
i=5 → stop

Count = **3**

---

## 💻 Code
```java
public static int countOccBetter(int[] arr, int x) {
    int n = arr.length;
    int i = 0;
    while (i < n && arr[i] < x) i++;
    if (i == n || arr[i] != x) return 0;

    int count = 0;
    while (i < n && arr[i] == x) {
        count++;
        i++;
    }
    return count;
}
```





# Count Occurrences (Optimal Approach — Binary Search)

## 🧠 Intuition (Hinglish)
Sorted array me BEST = **Binary Search for first & last occurrence**.

Formula:
```
count = lastOcc - firstOcc + 1
```

Trick:
- First occurrence = leftmost index jaha arr[mid] == x  
- Last occurrence = rightmost index jaha arr[mid] == x  
Agar x kahin nahi mila → return 0

Time: O(log n)  
Space: O(1)

---

## 📝 Dry Run
arr = [1,2,4,4,4,5,7], x = 4

First Occurrence search:
→ mid=3 → arr[3]=4 → ans=3 → go left  
→ mid=1 → arr[1]=2 <4 → go right  
→ mid=2 → arr[2]=4 → ans=2 → go left → STOP  
first = **2**

Last Occurrence search:
→ mid=3 → 4 → ans=3 → go right  
→ mid=4 → 4 → ans=4 → go right  
→ mid=5 → 5>4 → left  
Stop → last = **4**

count = 4 − 2 + 1 = **3**

---

## 💻 Code
```java
public static int countOccOptimal(int[] arr, int x) {
    int first = firstOcc(arr, x);
    if (first == -1) return 0;
    int last = lastOcc(arr, x);
    return last - first + 1;
}

private static int firstOcc(int[] arr, int x) {
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

private static int lastOcc(int[] arr, int x) {
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


