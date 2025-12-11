

# Remove Duplicate from Sorted Array (Brute Approach)

## 🧠 Intuition (Hinglish)
Brute force me hum simply ek **extra array** bana sakte hain aur har unique element ko usme store kar sakte hain.  
Sorted array me duplicates ek saath hote hain, toh bas check karna hota hai ki koi element **previous stored element se same** to nahi.

Isme hum array ko overwrite nahi karte — sirf unique elements ko naya array me rakh lete hain.

---

## 📝 Dry Run
Array = [1, 1, 2, 2, 3]

unique = []

- 1 → unique me add → [1]  
- next 1 → same as last unique → skip  
- 2 → add → [1, 2]  
- next 2 → skip  
- 3 → add → [1, 2, 3]  

Final unique array = [1, 2, 3]  
Count = 3

---

## 💻 Code
```java
public static int removeDuplicatesBrute(int[] arr) {
    ArrayList<Integer> list = new ArrayList<>();
    
    for (int i = 0; i < arr.length; i++) {
        if (i == 0 || arr[i] != arr[i - 1]) {
            list.add(arr[i]);
        }
    }
    
    // Copy back into array
    for (int i = 0; i < list.size(); i++) {
        arr[i] = list.get(i);
    }
    
    return list.size(); // number of unique elements
}
```


# Remove Duplicate from Sorted Array (Better Approach)

## 🧠 Intuition (Hinglish)
Better approach me hum **ek pointer unique elements ke liye** rakhte hain.  
Jab bhi hume koi new unique value milti hai, hum usko array me next unique position par store kar dete hain.

Ye brute se better hai kyunki extra list ka use nahi kar rahe.

---

## 📝 Dry Run
Array = [1, 1, 2, 2, 3]

i = 0 → arr[0] = 1  
uniqueIndex = 0

- j = 1 → 1 == 1 → skip  
- j = 2 → 2 != 1 → uniqueIndex = 1, arr[1] = 2  
- j = 3 → 2 == 2 → skip  
- j = 4 → 3 != 2 → uniqueIndex = 2, arr[2] = 3  

Final array (first 3 elements): [1, 2, 3]  
Count = 3

---

## 💻 Code
```java
public static int removeDuplicatesBetter(int[] arr) {
    int uniqueIndex = 0; 
    
    for (int j = 1; j < arr.length; j++) {
        if (arr[j] != arr[uniqueIndex]) {
            uniqueIndex++;
            arr[uniqueIndex] = arr[j];
        }
    }
    
    return uniqueIndex + 1;
}
```




# Remove Duplicate from Sorted Array (Optimal Approach)

## 🧠 Intuition (Hinglish)
Optimal approach me hum **two-pointer technique** use karte hain:

- i pointer → last unique element ko track karega  
- j pointer → array scan karega  
- Agar arr[j] ≠ arr[i] → new unique element mil gaya → i++ karke arr[i] = arr[j]

Isse hum:
✔ array ko inplace modify karte hain  
✔ koi extra space nahi lagta  
✔ single pass me kaam ho jata hai

---

## 📝 Dry Run
Array: [1, 1, 2, 2, 3]

i = 0 (pointing at 1)

- j = 1 → 1 == 1 → skip  
- j = 2 → 2 != 1 → i=1, arr[1]=2  
- j = 3 → 2 == 2 → skip  
- j = 4 → 3 != 2 → i=2, arr[2]=3  

Final array (unique part): [1, 2, 3]  
Count = i + 1 = **3**

---

## 💻 Code
```java
public static int removeDuplicatesOptimal(int[] arr) {
    int i = 0; 
    
    for (int j = 1; j < arr.length; j++) {
        if (arr[j] != arr[i]) {
            i++;
            arr[i] = arr[j];
        }
    }
    
    return i + 1;
}
```


