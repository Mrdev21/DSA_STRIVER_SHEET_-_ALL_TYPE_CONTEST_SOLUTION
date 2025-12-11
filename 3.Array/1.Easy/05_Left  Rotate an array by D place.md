

# Left Rotate an Array by k Places (Brute Approach)

## 🧠 Intuition (Hinglish)
Brute approach me hum simple hain: ek-ek karke array ko left rotate karenge, total k baar.  
Har single left-rotation me pehla element nikaal ke sab elements ko ek position left shift kar do aur pehle element ko end me daal do.  
Simple hai, par agar k bada hai to time zyada lagta hai (k * n).

---

## 📝 Dry Run
Array: [1, 2, 3, 4, 5], k = 2

**First single-left rotation:**
- take first = 1  
- shift → [2, 3, 4, 5, ?]  
- place first at end → [2, 3, 4, 5, 1]

**Second single-left rotation:**
- take first = 2  
- shift → [3, 4, 5, 1, ?]  
- place first at end → [3, 4, 5, 1, 2]

Final → [3, 4, 5, 1, 2]

### Time Complexity
O(n * k)  
### Space Complexity
O(1)

### 💻 Code
```java
public static void leftRotateBrute(int[] arr, int k) {
    if (arr == null || arr.length == 0) return;
    int n = arr.length;
    k = k % n;
    for (int times = 0; times < k; times++) {
        // single left rotation
        int first = arr[0];
        for (int i = 0; i < n - 1; i++) {
            arr[i] = arr[i + 1];
        }
        arr[n - 1] = first;
    }
}
```



# Left Rotate an Array by k Places (Better Approach)

## 🧠 Intuition (Hinglish)
Better approach me extra temporary array use karenge:  
Final rotated array ke index i par woh element aayega jo original me index (i + k) % n par tha.  
Isliye ek temp array me sab values place karke fir wapas original me copy kar denge.  
Time O(n) hai, space O(n).

---

## 📝 Dry Run
Array: [1, 2, 3, 4, 5], k = 2, n = 5

Compute temp:
- temp[0] = arr[(0 + 2) % 5] = arr[2] = 3  
- temp[1] = arr[(1 + 2) % 5] = arr[3] = 4  
- temp[2] = arr[(2 + 2) % 5] = arr[4] = 5  
- temp[3] = arr[(3 + 2) % 5] = arr[0] = 1  
- temp[4] = arr[(4 + 2) % 5] = arr[1] = 2

temp = [3, 4, 5, 1, 2] → copy back to arr

Final → [3, 4, 5, 1, 2]

### Time Complexity
O(n)  
### Space Complexity
O(n)

### 💻 Code
```java
public static void leftRotateBetter(int[] arr, int k) {
    if (arr == null || arr.length == 0) return;
    int n = arr.length;
    k = k % n;
    int[] temp = new int[n];
    for (int i = 0; i < n; i++) {
        temp[i] = arr[(i + k) % n];
    }
    // copy back
    for (int i = 0; i < n; i++) {
        arr[i] = temp[i];
    }
}
```



# Left Rotate an Array by k Places (Optimal Approach)

## 🧠 Intuition (Hinglish)
Optimal (in-place) trick: 3 reversals.  
Left rotate by k ka matlab hai pehle k elements ko front se nikal ke end me le jana.  
Steps:
1. k = k % n (normalize)  
2. Reverse first k elements (0 .. k-1)  
3. Reverse remaining elements (k .. n-1)  
4. Reverse whole array (0 .. n-1)

Example se dekho: [1,2,3,4,5], k=2  
- reverse(0..1) → [2,1,3,4,5]  
- reverse(2..4) → [2,1,5,4,3]  
- reverse(0..4) → [3,4,5,1,2]

Ye O(n) time aur O(1) extra space deta hai.

---

## 📝 Dry Run
Array: [1, 2, 3, 4, 5], k = 2, n = 5

1) k % n = 2  
2) reverse(0..1): [2, 1, 3, 4, 5]  
3) reverse(2..4): [2, 1, 5, 4, 3]  
4) reverse(0..4): [3, 4, 5, 1, 2]

Final → [3, 4, 5, 1, 2]

### Time Complexity
O(n)  
### Space Complexity
O(1)

### 💻 Code
```java
public static void leftRotateOptimal(int[] arr, int k) {
    if (arr == null || arr.length == 0) return;
    int n = arr.length;
    k = k % n;
    if (k == 0) return;

    // helper reverse
    reverse(arr, 0, k - 1);
    reverse(arr, k, n - 1);
    reverse(arr, 0, n - 1);
}

private static void reverse(int[] arr, int l, int r) {
    while (l < r) {
        int tmp = arr[l];
        arr[l] = arr[r];
        arr[r] = tmp;
        l++;
        r--;
    }
}
```
