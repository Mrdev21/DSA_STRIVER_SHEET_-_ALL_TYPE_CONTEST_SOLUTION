
# Sort an array of 0's, 1's and 2's (Brute Approach)

## 🧠 Intuition (Hinglish)
Brute force me hum simply array ko **sort()** kar dete hain.  
Kyuki numbers sirf 0, 1, 2 hain, sorting se automatically correct order mil jata hai.  
Simple approach, lekin sorting O(n log n) time deti hai.

---

## 📝 Dry Run
Array: [2, 0, 1, 2, 1]

After sorting → [0, 1, 1, 2, 2]

---

## 💻 Code
```java
public static void sort012Brute(int[] arr) {
    Arrays.sort(arr);
}
```


# Sort an array of 0's, 1's and 2's (Better Approach)

## 🧠 Intuition (Hinglish)
Better approach me hum **counting method** use karte hain:
- Count how many 0s, 1s, 2s are in the array  
- Pehle 0s likho, fir 1s, fir 2s  
Isme comparisons nahi hote, sirf counting hoti hai.

Time O(n), space O(1).

---

## 📝 Dry Run
Array: [2, 0, 1, 2, 1]

Count:
0 → 1  
1 → 2  
2 → 2  

Rebuild array:
[0, 1, 1, 2, 2]

---

## 💻 Code
```java
public static void sort012Better(int[] arr) {
    int count0 = 0, count1 = 0, count2 = 0;

    for (int x : arr) {
        if (x == 0) count0++;
        else if (x == 1) count1++;
        else count2++;
    }

    int index = 0;
    while (count0-- > 0) arr[index++] = 0;
    while (count1-- > 0) arr[index++] = 1;
    while (count2-- > 0) arr[index++] = 2;
}
```



# Sort an array of 0's, 1's and 2's (Optimal Approach)

## 🧠 Intuition (Hinglish)
Optimal solution me hum **three-pointer Dutch National Flag Algorithm** use karte hain:
- low → 0s area ka end  
- mid → current element  
- high → 2s area ka start  

Rules:
- arr[mid] == 0 → swap with low, low++, mid++  
- arr[mid] == 1 → mid++  
- arr[mid] == 2 → swap with high, high-- (mid move mat karo)

Ye O(n) time, O(1) space aur one-pass solution deta hai.

---

## 📝 Dry Run
Array: [2, 0, 1, 2, 1]

low=0, mid=0, high=4

- arr[mid]=2 → swap(mid,high) → [1,0,1,2,2], high=3  
- arr[mid]=1 → mid=1  
- arr[mid]=0 → swap(mid,low) → [0,1,1,2,2], low=1, mid=2  
- arr[mid]=1 → mid=3  
- arr[mid]=2 → swap(mid,high) → [0,1,1,2,2], high=2 → done

Final → [0, 1, 1, 2, 2]

---

## 💻 Code
```java
public static void sort012Optimal(int[] arr) {
    int low = 0, mid = 0, high = arr.length - 1;

    while (mid <= high) {
        if (arr[mid] == 0) {
            int temp = arr[low];
            arr[low] = arr[mid];
            arr[mid] = temp;
            low++;
            mid++;
        } 
        else if (arr[mid] == 1) {
            mid++;
        } 
        else {
            int temp = arr[mid];
            arr[mid] = arr[high];
            arr[high] = temp;
            high--;
        }
    }
}
```


