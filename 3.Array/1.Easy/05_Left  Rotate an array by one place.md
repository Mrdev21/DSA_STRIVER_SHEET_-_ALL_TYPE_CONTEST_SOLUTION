

# Left Rotate an Array by One Place (Brute Approach)

## 🧠 Intuition (Hinglish)
Brute force me hum simple ek new array banayenge.  
- Original array ka pehla element → last position par chala jayega  
- Baaki saare elements ek index left shift ho jayenge  
Extra O(n) space lagta hai — but logic simple hai.

---

## 📝 Dry Run
arr = [1, 2, 3, 4, 5]

new array steps:
- new[0] = arr[1] → 2  
- new[1] = arr[2] → 3  
- new[2] = arr[3] → 4  
- new[3] = arr[4] → 5  
- new[4] = arr[0] → 1  

Result → **[2, 3, 4, 5, 1]**

---

## 💻 Code
```java
public static int[] leftRotateBrute(int[] arr) {
    int n = arr.length;
    int[] res = new int[n];

    for (int i = 1; i < n; i++) {
        res[i - 1] = arr[i];
    }
    res[n - 1] = arr[0];

    return res;
}
```


# Left Rotate an Array by One Place (Better Approach)

## 🧠 Intuition (Hinglish)
Better approach me hum ek *temporary variable* me first element store kar lete hain.  
Phir pura array ek position left shift kar dete hain.  
Aakhir me last position par temp daal dete hain.  
Isme extra space O(1) lagta hai.

---

## 📝 Dry Run
arr = [1, 2, 3, 4, 5]

temp = 1  
Shift:
- arr[0] = arr[1] → 2  
- arr[1] = arr[2] → 3  
- arr[2] = arr[3] → 4  
- arr[3] = arr[4] → 5  
Put back:
- arr[4] = temp → 1  

Result → **[2, 3, 4, 5, 1]**

---

## 💻 Code
```java
public static void leftRotateBetter(int[] arr) {
    int n = arr.length;
    int temp = arr[0];

    for (int i = 1; i < n; i++) {
        arr[i - 1] = arr[i];
    }
    arr[n - 1] = temp;
}
```

# Left Rotate an Array by One Place (Optimal Approach)

## 🧠 Intuition (Hinglish)
Optimal trick me hum **reverse technique** use kar sakte the**,**  
but single rotation ke liye best = **swap-based O(1) shift**, same Better approach.  
Is problem ke liye actual “Optimal” = **O(n) time, O(1) space**, which is already achieved.

So Optimal = Better → cleanest version.

---

## 📝 Dry Run
arr = [10, 20, 30]

temp = 10  
shift → [20, 30, _]  
last = temp → 10  

Result → **[20, 30, 10]**

---

## 💻 Code
```java
public static void leftRotateOptimal(int[] arr) {
    int n = arr.length;
    int temp = arr[0];

    for (int i = 1; i < n; i++) {
        arr[i - 1] = arr[i];
    }
    arr[n - 1] = temp;
}
```




