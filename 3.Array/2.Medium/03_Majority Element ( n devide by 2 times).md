


# Majority Element (n/2 times) — Brute Approach

## 🧠 Intuition (Hinglish)
Brute force me hum har element ka count nikalenge by scanning the whole array.  
Agar kisi number ka count > n/2 ho gaya → wahi majority element hai.  
Slow hai kyunki har element ke liye poora array traverse hota hai (O(n²)).

---

## 📝 Dry Run
Array: [2, 2, 1, 1, 2]

Check 2 → count = 3 → 3 > 5/2 → **majority = 2**  
Check 2 again → same  
Check 1 → count = 2 → not majority

Final → **2**

---

## 💻 Code
```java
public static int majorityBrute(int[] arr) {
    int n = arr.length;

    for (int i = 0; i < n; i++) {
        int count = 0;

        for (int j = 0; j < n; j++) {
            if (arr[j] == arr[i]) count++;
        }

        if (count > n / 2) return arr[i];
    }

    return -1;
}
```



# Majority Element (n/2 times) — Better Approach

## 🧠 Intuition (Hinglish)
Better method me hum **HashMap** use karte hain frequency count rakhne ke liye.  
Map me har element ka count store hota hai.  
Jiska count > n/2 hota hai → woh majority element.

Yeh O(n) time me kaam karta hai but O(n) extra space use karta hai.

---

## 📝 Dry Run
Array: [2, 2, 1, 1, 2]

Map:
2 → count = 3  
1 → count = 2  

3 > n/2 (2) → **majority = 2**

---

## 💻 Code
```java
public static int majorityBetter(int[] arr) {
    HashMap<Integer, Integer> map = new HashMap<>();
    int n = arr.length;

    for (int x : arr) {
        map.put(x, map.getOrDefault(x, 0) + 1);
    }

    for (int key : map.keySet()) {
        if (map.get(key) > n / 2) return key;
    }

    return -1;
}
```






# Majority Element (n/2 times) — Optimal Approach

## 🧠 Intuition (Hinglish)
Optimal solution me **Moore’s Voting Algorithm** use hota hai:

Logic:
- Majority element total me half se zyada hota hai,  
  isliye agar hum pairing/destroying technique use karein,  
  majority element last tak survive karta hai.
- Ek candidate aur ek count rakhte hain:
  - agar count = 0 → candidate = arr[i]  
  - arr[i] == candidate → count++  
  - arr[i] != candidate → count--  

End me jo candidate bachega woh majority hoga.

---

## 📝 Dry Run
Array: [2, 2, 1, 1, 2]

Start:
candidate = 2, count = 1  
Next 2 → count = 2  
Next 1 → count = 1  
Next 1 → count = 0  
Next 2 → count = 1 → candidate = 2

Final → **2**

---

## 💻 Code
```java
public static int majorityOptimal(int[] arr) {
    int candidate = 0;
    int count = 0;

    for (int num : arr) {
        if (count == 0) {
            candidate = num;
        }
        if (num == candidate) count++;
        else count--;
    }

    return candidate; 
}
```


