
# Single Element in a Sorted Array (Brute Approach)

## 🧠 Intuition (Hinglish)
Brute force me har element ki frequency count kar do (using simple scan).  
Ya pair-wise check:  
Sorted array me duplicates always appear in pairs: (x,x), (y,y), ...  
Toh brute me check karo:  
- agar arr[i] != arr[i+1] → arr[i] unique hoga  
- warna i += 2

Time: O(n), Space: O(1)

---

## 📝 Dry Run
arr = [1,1,2,3,3,4,4]

i=0 → 1 == 1 → i += 2 → i=2  
i=2 → 2 != 3 → unique = **2**  

---

## 💻 Code
```java
public static int singleElementBrute(int[] arr) {
    int n = arr.length;
    for (int i = 0; i < n - 1; i += 2) {
        if (arr[i] != arr[i + 1]) {
            return arr[i];
        }
    }
    return arr[n - 1]; // last element must be unique
}
```


# Single Element in a Sorted Array (Better Approach — XOR)

## 🧠 Intuition (Hinglish)
Better approach = **XOR trick**  
Because:  
- x ^ x = 0  
- 0 ^ y = y  
- XOR is commutative and associative  

Toh agar sab elements XOR kar do, duplicates cancel ho jayenge, aur unique element bach jayega.

Time: O(n), Space: O(1)

---

## 📝 Dry Run
arr = [1,1,2,3,3,4,4]

xor = 0  
xor ^= 1 → 1  
xor ^= 1 → 0  
xor ^= 2 → 2  
xor ^= 3 → 1  
xor ^= 3 → 2  
xor ^= 4 → 6  
xor ^= 4 → 2  

Final → **2**

---

## 💻 Code
```java
public static int singleElementBetter(int[] arr) {
    int xor = 0;
    for (int v : arr) xor ^= v;
    return xor;
}
```



# Single Element in a Sorted Array (Optimal Approach — Binary Search, O(log n))

## 🧠 Intuition (Hinglish)
Sorted array me duplicates **always appear in pairs**:  
- pair start index even (0,2,4...)  
- pair end index odd (1,3,5...)

Lekin **jis point pe unique element aata hai, uske baad se pairing pattern break ho jata hai**.

Binary search trick:
- mid nikaalo  
- mid ko even position par laane ke liye:
  - agar mid odd hai → mid--  
- agar arr[mid] == arr[mid+1] → left half perfect pairs → answer right side me  
- else → answer left half me (mid itself could be answer)  

Time: O(log n)  
Space: O(1)

---

## 📝 Dry Run
arr = [1,1,2,3,3,4,4]

low=0, high=6  
mid=3 → odd → mid-- = 2  
arr[2]=2, arr[3]=3 → not equal → answer left side → high = 2  

low=0, high=2  
mid=1 (odd) → mid-- = 0  
arr[0]=1, arr[1]=1 → equal → answer right → low = mid+2 = 2  

low=2, high=2  
mid=2 → **unique = 2**

---

## 💻 Code
```java
public static int singleElementOptimal(int[] arr) {
    int low = 0, high = arr.length - 1;

    while (low < high) {
        int mid = low + (high - low) / 2;

        // ensure mid is even
        if (mid % 2 == 1) mid--;

        if (arr[mid] == arr[mid + 1]) {
            // left half is perfectly paired -> answer in right half
            low = mid + 2;
        } else {
            // pairing broken -> answer at mid or left
            high = mid;
        }
    }
    return arr[low];
}
```


