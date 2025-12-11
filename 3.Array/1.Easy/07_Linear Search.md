
# Linear Search (Brute Approach)

## 🧠 Intuition (Hinglish)
Brute force me hum bilkul basic approach follow karte hain:  
Array ke har element ko target ke saath compare karte jao.  
Jaha match mil jaye, wahi return kar do.  
Ye sabse straightforward method hai.

---

## 📝 Dry Run
Array: [4, 2, 7, 1, 9], target = 1

- i = 0 → 4 != 1  
- i = 1 → 2 != 1  
- i = 2 → 7 != 1  
- i = 3 → 1 == 1 → **found at index 3**

Output → 3

---

## 💻 Code
```java
public static int linearSearchBrute(int[] arr, int target) {
    for (int i = 0; i < arr.length; i++) {
        if (arr[i] == target) return i;
    }
    return -1;
}
```

# Linear Search (Better Approach)

## 🧠 Intuition (Hinglish)
Better approach me hum same traversal karte hain,  
lekin jaise hi target milta hai turant loop break kar dete hain.  
Performance same lagti hai but real-time me unnecessary comparisons ruk jaate hain.

---

## 📝 Dry Run
Array: [4, 2, 7, 1, 9], target = 7

- i = 0 → 4 != 7  
- i = 1 → 2 != 7  
- i = 2 → 7 == 7 → **break**  
Index = 2

---

## 💻 Code
```java
public static int linearSearchBetter(int[] arr, int target) {
    int i = 0;
    while (i < arr.length) {
        if (arr[i] == target) return i;
        i++;
    }
    return -1;
}
```


# Linear Search (Optimal Approach)

## 🧠 Intuition (Hinglish)
Optimal linear search me hum code ko short, readable aur efficient banate hain:  
Enhanced for-loop use kar sakte hain jisme index track ho.  
Ye minimal overhead ke saath sabse clean approach hota hai.

---

## 📝 Dry Run
Array: [4, 2, 7, 1, 9], target = 9

Loop:
- idx=0 → 4  
- idx=1 → 2  
- idx=2 → 7  
- idx=3 → 1  
- idx=4 → 9 → **match**

Return → 4

---

## 💻 Code
```java
public static int linearSearchOptimal(int[] arr, int target) {
    int index = 0;
    for (int num : arr) {
        if (num == target) return index;
        index++;
    }
    return -1;
}
```



