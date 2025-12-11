
# Find Minimum in Rotated Sorted Array (Brute Approach — Linear Scan)

## 🧠 Intuition (Hinglish)
Brute method me hum simply pura array scan karenge aur **smallest element** find kar lenge.  
Rotation ka koi bachcha-nahi use hota — bas normal min search.

Time: O(n)  
Space: O(1)

---

## 📝 Dry Run
arr = [4,5,6,7,0,1,2]

Traverse:
min = 4  
5 → no  
6 → no  
7 → no  
0 → yes → min = 0  
1 → no  
2 → no  

Answer → **0**

---

## 💻 Code
```java
public static int findMinBrute(int[] arr) {
    int min = Integer.MAX_VALUE;
    for (int v : arr) {
        min = Math.min(min, v);
    }
    return min;
}
```


# Find Minimum in Rotated Sorted Array (Better Approach — Check Pivot by Linear Scan)

## 🧠 Intuition (Hinglish)
Better idea:  
Rotated sorted array me **minimum element = pivot**.  
Pivot wahi hota hai jaha `arr[i] > arr[i+1]`.  
Jab aisa mile → `arr[i+1]` hi minimum hai.

Still O(n) worst-case, but generally early mil jaata hai.

---

## 📝 Dry Run
arr = [7,8,9,1,2,3]

i=0 → 7 < 8  
i=1 → 8 < 9  
i=2 → 9 > 1 → break → **min = 1**

---

## 💻 Code
```java
public static int findMinBetter(int[] arr) {
    int n = arr.length;
    for (int i = 0; i < n - 1; i++) {
        if (arr[i] > arr[i + 1]) {
            return arr[i + 1];
        }
    }
    return arr[0]; // already sorted, no rotation
}
```



# Find Minimum in Rotated Sorted Array (Optimal Approach — Binary Search)

## 🧠 Intuition (Hinglish)
Optimal = **Binary Search**  
Sorted rotated array ki ek important property:  
- Agar `arr[low] <= arr[high]` → pura array sorted → first element hi minimum  
- Mid compare with right:
  - agar `arr[mid] > arr[high]` → minimum right side me  
  - agar `arr[mid] <= arr[high]` → minimum left side me (including mid)

Binary search se O(log n) me pivot / minimum mil jaata hai.

---

## 📝 Dry Run
arr = [4,5,6,7,0,1,2]

low=0, high=6  
mid=3 → arr[3]=7 > arr[6]=2 → min right side → low=4  

low=4, high=6  
mid=5 → arr[5]=1 <= arr[6]=2 → min left side → high=5  

low=4, high=5  
mid=4 → arr[4]=0 <= arr[5]=1 → min left → high=4  

low=4, high=4 → loop end → **min = arr[4] = 0**

---

## 💻 Code
```java
public static int findMinOptimal(int[] arr) {
    int low = 0, high = arr.length - 1;

    while (low < high) {
        int mid = low + (high - low) / 2;

        if (arr[mid] > arr[high]) {
            // minimum is in right half
            low = mid + 1;
        } else {
            // minimum is in left half including mid
            high = mid;
        }
    }
    return arr[low]; // low == high
}
```



