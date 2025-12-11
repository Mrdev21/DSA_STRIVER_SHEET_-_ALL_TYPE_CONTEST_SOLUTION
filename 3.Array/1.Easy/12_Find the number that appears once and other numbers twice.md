

# Find the Number That Appears Once (Brute Approach)

## 🧠 Intuition (Hinglish)
Brute approach me hum har element ko uthakar poore array me count kar sakte hain.  
Agar koi element ka count 1 nikla → wahi answer.  
Yeh simplest but slowest method hai (O(n²)).

---

## 📝 Dry Run
Array: [2, 3, 2, 4, 3]

Check 2 → count = 2  
Check 3 → count = 2  
Check 2 → count = 2  
Check 4 → count = 1 → **answer = 4**

---

## 💻 Code
```java
public static int singleNumberBrute(int[] arr) {
    for (int i = 0; i < arr.length; i++) {
        int count = 0;
        for (int j = 0; j < arr.length; j++) {
            if (arr[i] == arr[j]) count++;
        }
        if (count == 1) return arr[i];
    }
    return -1;
}
```





# Find the Number That Appears Once (Better Approach)

## 🧠 Intuition (Hinglish)
Better method me hum ek **HashMap** ya **HashSet** use kar sakte hain.  
- Map me har element ki frequency store kar do  
- Jiska count 1 ho → wahi unique number  

Yeh O(n) time me kaam karta hai par thoda extra space use karta hai.

---

## 📝 Dry Run
Array: [2, 3, 2, 4, 3]

Map banega:
- 2 → 2 times  
- 3 → 2 times  
- 4 → 1 time

Jiska count 1 → **4**

---

## 💻 Code
```java
public static int singleNumberBetter(int[] arr) {
    HashMap<Integer, Integer> map = new HashMap<>();

    for (int x : arr) {
        map.put(x, map.getOrDefault(x, 0) + 1);
    }

    for (int key : map.keySet()) {
        if (map.get(key) == 1) return key;
    }

    return -1;
}
```



# Find the Number That Appears Once (Optimal Approach)

## 🧠 Intuition (Hinglish)
Optimal trick XOR ka hota hai:

✔ Same number XOR same number = 0  
✔ 0 XOR x = x  
✔ XOR is commutative + associative  

Matlab:  
Agar saare duplicate numbers pair me hain,  
toh unka XOR **cancel** ho jayega → 0 ban jayega.  
Sirf jo element single hai → wahi bach jayega.

Ye pure array ko XOR karne se directly answer milta hai.

---

## 📝 Dry Run
Array: [2, 3, 2, 4, 3]

XOR steps:
res = 0  
res ^= 2 → 2  
res ^= 3 → 1  
res ^= 2 → 3  
res ^= 4 → 7  
res ^= 3 → **4**

Final answer = **4**

---

## 💻 Code
```java
public static int singleNumberOptimal(int[] arr) {
    int xor = 0;

    for (int num : arr) {
        xor ^= num;
    }

    return xor;
}
```
