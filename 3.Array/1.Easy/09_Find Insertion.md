

# Find Insertion Position in Sorted Array (Brute Approach)

## 🧠 Intuition (Hinglish)
Brute method me hum simply target ko array me insert karne ki jagah dhoondne ke liye  
**har element ko left se right** check karte hain.  
Jaha pehli baar koi element target se bada mil jaye → wahi index insertion position hai.

Agar end tak koi bada element nahi mila → target end me jayega.

---

## 📝 Dry Run
Array = [1, 3, 5, 6]  
Target = 5

i = 0 → 1 < 5  
i = 1 → 3 < 5  
i = 2 → 5 == 5 → **insert at index 2**

Output → 2

---

## 💻 Code
```java
public static int insertionBrute(int[] arr, int target) {
    for (int i = 0; i < arr.length; i++) {
        if (arr[i] >= target) return i;
    }
    return arr.length;
}
```



# Find Insertion Position in Sorted Array (Better Approach)

## 🧠 Intuition (Hinglish)
Better approach me hum linear traversal hi karte hain  
par jaise hi first element target se bada mile →  
**turant break** kar dete hain.

Same O(n), but cleaner.

---

## 📝 Dry Run
Array = [1, 3, 5, 6]  
Target = 2

i = 0 → 1 < 2  
i = 1 → 3 > 2 → **insert at index 1**

Output → 1

---

## 💻 Code
```java
public static int insertionBetter(int[] arr, int target) {
    int i = 0;
    while (i < arr.length) {
        if (arr[i] >= target) return i;
        i++;
    }
    return arr.length;
}
```



# Find Insertion Position in Sorted Array (Optimal Approach)

## 🧠 Intuition (Hinglish)
Optimal method me hum **binary search** use karte hain.  
Sorted array hai, toh hume har element sequentially check karne ki zarurat nahi.  
Binary search me:

- If target found → return index  
- If not found → jaha `low` finally rukta hai, wahi insertion position hoti hai

Ye fastest solution hai.

---

## 📝 Dry Run
Array = [1, 3, 5, 6]  
Target = 4

low = 0  
high = 3

mid = 1 → arr[1] = 3 < 4 → low = 2  
mid = 2 → arr[2] = 5 > 4 → high = 1  

Loop ends → low = 2 → **insert at index 2**

Output → 2

---

## 💻 Code
```java
public static int insertionOptimal(int[] arr, int target) {
    int low = 0, high = arr.length - 1;

    while (low <= high) {
        int mid = low + (high - low) / 2;

        if (arr[mid] == target) return mid;
        else if (arr[mid] < target) low = mid + 1;
        else high = mid - 1;
    }

    return low;  // final insertion index
}
```




