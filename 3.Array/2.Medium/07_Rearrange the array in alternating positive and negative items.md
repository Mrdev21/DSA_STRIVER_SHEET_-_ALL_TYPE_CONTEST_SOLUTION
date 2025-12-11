
# Rearrange Array in Alternating Positive and Negative (Brute Approach)

## 🧠 Intuition (Hinglish)
Brute force me hum do separate lists bana lete hain — ek positives ke liye aur ek negatives ke liye.  
Phir new array banake un lists ke elements ko alternating fashion me fill kar dete hain.  
Simple aur direct approach, but extra space lagta hai.

---

## 📝 Dry Run
Array: [1, -2, 3, -4, 5, -6]

positives = [1, 3, 5]  
negatives = [-2, -4, -6]

Fill alternating:
index 0 → positive → 1  
index 1 → negative → -2  
index 2 → positive → 3  
index 3 → negative → -4  
index 4 → positive → 5  
index 5 → negative → -6  

Result → **[1, -2, 3, -4, 5, -6]**

---

## 💻 Code
```java
public static int[] rearrangeBrute(int[] arr) {
    ArrayList<Integer> pos = new ArrayList<>();
    ArrayList<Integer> neg = new ArrayList<>();

    for (int x : arr) {
        if (x >= 0) pos.add(x);
        else neg.add(x);
    }

    int i = 0, p = 0, n = 0;

    while (p < pos.size() && n < neg.size()) {
        arr[i++] = pos.get(p++);
        arr[i++] = neg.get(n++);
    }

    while (p < pos.size()) arr[i++] = pos.get(p++);
    while (n < neg.size()) arr[i++] = neg.get(n++);

    return arr;
}
```




# Rearrange Array in Alternating Positive and Negative (Better Approach)

## 🧠 Intuition (Hinglish)
Better approach me hum ek **result array** banate hain aur scan karte waqt:  
- Positive mile → even index (0,2,4…) me daalo  
- Negative mile → odd index (1,3,5…) me daalo  
Agar koi type khatam ho jaye, baaki ke elements end me rakh dete hain.

Isme ek hi pass me values place hoti hain, extra O(n) space use hota hai.

---

## 📝 Dry Run
Array: [1, -2, 3, -4, 5]

Even index → positives  
Odd index → negatives

res[0] = 1  
res[1] = -2  
res[2] = 3  
res[3] = -4  
res[4] = 5  

Result → **[1, -2, 3, -4, 5]**

---

## 💻 Code
```java
public static int[] rearrangeBetter(int[] arr) {
    int[] res = new int[arr.length];
    int posIndex = 0;  // even index
    int negIndex = 1;  // odd index

    for (int x : arr) {
        if (x >= 0) {
            if (posIndex < arr.length) {
                res[posIndex] = x;
                posIndex += 2;
            }
        } else {
            if (negIndex < arr.length) {
                res[negIndex] = x;
                negIndex += 2;
            }
        }
    }

    return res;
}
```



# Rearrange Array in Alternating Positive and Negative (Optimal Approach)

## 🧠 Intuition (Hinglish)
Optimal approach me pehle hum array ko **partition** karte hain (like quicksort partition):
- Saare negatives left side  
- Saare positives right side  

Phir ek pointer negative start pe aur ek pointer positive start pe set karo.  
Ab alternate swapping se negative aur positive ko correct jagah rakhte jao.

Isse rearrangement **in-place** hota hai, extra space O(1), time O(n).

---

## 📝 Dry Run
Array: [1, -2, 3, -4, 5, -6]

1) Partition → negatives left, positives right  
After partition (example order): [-2, -4, -6, 1, 3, 5]  
neg = 0  
pos = index of first positive = 3  

2) Alternating placement by swapping:
swap index 1 with pos=3 → [-2, 1, -6, -4, 3, 5]  
neg=2, pos=4  
swap index 3 with pos=4 → [-2, 1, -6, 3, -4, 5]  
neg=4, pos=5  
swap index 5 with pos=5 → [-2, 1, -6, 3, -4, 5]

Final (starting with negative or positive depends on convention) →  
**[-2, 1, -6, 3, -4, 5]**

---

## 💻 Code
```java
public static void rearrangeOptimal(int[] arr) {
    int n = arr.length;
    int i = -1;

    // partition step
    for (int j = 0; j < n; j++) {
        if (arr[j] < 0) {
            i++;
            int temp = arr[i];
            arr[i] = arr[j];
            arr[j] = temp;
        }
    }

    int neg = 0;      // start of negatives
    int pos = i + 1;  // start of positives

    // alternate swapping
    while (neg < pos && pos < n && arr[neg] < 0) {
        int temp = arr[neg];
        arr[neg] = arr[pos];
        arr[pos] = temp;

        neg += 2;
        pos++;
    }
}
```
