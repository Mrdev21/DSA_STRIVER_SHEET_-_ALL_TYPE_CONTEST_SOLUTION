

# Brute Force, No Sorting

## 🧠 Intuition (Hinglish)
Sorting allowed nahi hai, isliye brute force me hum har element ko “candidate” maan kar usse compare karenge baaki sab elements se.  
Agar koi number us candidate se bada milta hai to woh second largest nahi ho sakta.  
Is tarah hum check kar sakte hain ki kaunsa number largest se chota par sabse bada hai.

---

## ❌ Brute Force Approach
- Pehle largest element find karo (O(n))
- Fir har element ko check karo aur verify karo ki:
  - Woh largest se chota ho  
  - Aur koi dusra element usse bada na ho  
- Jo is condition ko satisfy kare → second largest

### Time Complexity
O(n²)

---

## 📝 Dry Run

Array: [10, 4, 8, 15, 7]

Largest = 15

Check each element:
- 10 → largest se chota ✔  
  - Compare with all others → koi 10 se bada? Yes (15) → valid  
  - Isse pehle koi second largest nahi tha → secondLargest = 10  
- 4 → chota ✔  
  - Compare → 4 se bade bohot hain → ignore  
- 8 → chota ✔  
  - Compare → only 15 > 8 → valid  
  - Compare with current secondLargest (10) → 8 < 10 → ignore  
- 15 → skip (largest)  
- 7 → ignore (less than 10)

Final second largest = **10**

---

## 💻 Code
```java
public static int secondLargestBrute(int[] arr) {
    // Step 1: find largest
    int largest = arr[0];
    for (int num : arr) {
        if (num > largest) largest = num;
    }

    // Step 2: brute check second largest
    int secondLargest = Integer.MIN_VALUE;

    for (int i = 0; i < arr.length; i++) {
        int candidate = arr[i];

        if (candidate == largest) continue;

        boolean isSecondLargest = true;

        for (int j = 0; j < arr.length; j++) {
            if (arr[j] > candidate && arr[j] < largest) {
                isSecondLargest = false;
                break;
            }
        }

        if (isSecondLargest && candidate > secondLargest) {
            secondLargest = candidate;
        }
    }

    return secondLargest;
}
```



# Second Largest Element in an Array (Better Approach)

## 🧠 Intuition (Hinglish)
Is approach me hum do baar array traverse karenge:  
1. Pehle pass me largest element nikal lo  
2. Dusre pass me largest ko skip karke sabse bada element dhoond lo  
Ye O(n) + O(n) = O(n) approach hai.

---

## 🔄 Better Approach
- First pass → largest element find karo  
- Second pass → largest ke alawa sabse bada number dhoondo

### Time Complexity
O(n)

---

## 📝 Dry Run

Array: [10, 4, 8, 15, 7]

**Pass 1 → Find largest**  
→ largest = 15

**Pass 2 → Find second largest**  
- 10 → valid candidate → secondLargest = 10  
- 4 → ignore  
- 8 → ignore (8 < 10)  
- 15 → skip  
- 7 → ignore  

Final second largest = **10**

---

## 💻 Code
```java
public static int secondLargestBetter(int[] arr) {
    int largest = arr[0];

    // first pass: find largest
    for (int num : arr) {
        if (num > largest) largest = num;
    }

    // second pass: find second largest
    int secondLargest = Integer.MIN_VALUE;

    for (int num : arr) {
        if (num != largest && num > secondLargest) {
            secondLargest = num;
        }
    }

    return secondLargest;
}
```



# Second Largest Element in an Array (Optimal Approach)

## 🧠 Intuition (Hinglish)
Optimal approach me hum sirf ek hi pass me largest & second largest dono track karenge.

Jab bhi koi naya number:
- largest se bada ho →  
  secondLargest = largest  
  largest = num  
- warna agar woh secondLargest se bada ho →  
  secondLargest update kar do

Isse hum bina sorting aur bina extra passes ke answer lete hain.

---

## ⚡ Optimal Approach
- Ek hi traversal  
- largest & secondLargest track karo  
- O(n) time, O(1) space

### Time Complexity
O(n)

---

## 📝 Dry Run

Array: [10, 4, 8, 15, 7]

Init:  
largest = -∞  
secondLargest = -∞

- 10 → largest = 10  
- 4 → secondLargest = 4  
- 8 → secondLargest = 8  
- 15 → secondLargest = 10, largest = 15  
- 7 → ignore  

Final → secondLargest = **10**

---

## 💻 Code
```java
public static int secondLargestOptimal(int[] arr) {
    int largest = Integer.MIN_VALUE;
    int secondLargest = Integer.MIN_VALUE;

    for (int num : arr) {

        if (num > largest) {
            secondLargest = largest;
            largest = num;
        }

        else if (num > secondLargest && num != largest) {
            secondLargest = num;
        }
    }

    return secondLargest;
}
```
