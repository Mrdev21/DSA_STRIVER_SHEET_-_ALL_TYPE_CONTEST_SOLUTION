
# Brute Force-

## 🧠 Intuition (Hinglish)
==Is approach me hum soch rahe:== 
=="Sabse bada element chahiye? To poora array sort kar do, sab automatically arrange ho jayenge aur last wala element biggest hoga."==

Sorting se elements increasing order me aa jate hain, isliye last index par maximum hota hai.

---

## ❌ Brute Force Approach
- Array ko sort karo (ascending).
- Last element return kar do.

### Time Complexity
*O(n log n)*

---
## 📝 Dry Run

Example: [3, 5, 2, 9, 1]

Sorted array → [1, 2, 3, 5, 9]  
Largest → 9

## 💻 Code
```java
public static int largestBrute(int[] arr) {
    Arrays.sort(arr);
    return arr[arr.length - 1];
}
```

---












# Better Approach-

## 🧠 Intuition (Hinglish)

==Yaha hum sorting nahi kar rahe.==  
==Hum simple compare kar ke max nikal lenge.==  
=="Ek number ko current winner banao aur baaki sab contestants se compare karte jao.”==

==Jitna bada number milega, woh winner ban jayega.==

---

## 🔄 Better Approach

- max = arr[0]
- Har element se compare karte jao
- If bigger → max update

### Time Complexity

O(n)

---
## 📝 Dry Run

Array: [3, 5, 2, 9, 1]

|Step|i|arr[i]|max_before|max_after|
|---|---|---|---|---|
|Init|0|3|—|3|
|1|1|5|3|5|
|2|2|2|5|5|
|3|3|9|5|9|
|4|4|1|9|9|

Final → **9**

## 💻 Code
```Java
public static int largestBetter(int[] arr) {
    int max = arr[0]; 
    for (int i = 1; i < arr.length; i++) {
        if (arr[i] > max) {
            max = arr[i];
        }
    }
    return max;
}
```

---



















# Optimal Approach-

## 🧠 Intuition (Hinglish)
==Optimal basically clean version of better approach.==
==Enhanced for-loop se code short aur readable ban jata hai.==
==Same logic: har element check → bigger mile to max update.==


---
## ⚡ Optimal Approach
- Single pass
- For-each loop
- O(n) time, O(1) space

### Time Complexity
O(n)

---
## 📝 Dry Run
Array: [3, 5, 2, 9, 1]

- num = 3 → max = 3  
- num = 5 → max = 5  
- num = 2 → no change  
- num = 9 → max = 9  
- num = 1 → no change  

Final → 9

## 💻 Code
```java
public static int largestOptimal(int[] arr) {
    int max = arr[0];
    for (int num : arr) {
        if (num > max) {
            max = num;
        }
    }
    return max;
}
```

---


