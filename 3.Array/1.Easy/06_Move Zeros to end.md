
# Move Zeros to End (Brute Approach)

## 🧠 Intuition (Hinglish)
Brute force me hum simply **non-zero elements ko ek new array/list me store** kar sakte hain.  
Phir end me jitne zeros missing hain, utne zeros append kar denge.  
Sorted nahi karna, bas non-zero + zero order maintain karna hai.

Ye simple hai par extra space use karta hai.

---

## 📝 Dry Run
Array: [0, 1, 0, 3, 12]

nonZero = []

- 0 → skip  
- 1 → add → [1]  
- 0 → skip  
- 3 → add → [1, 3]  
- 12 → add → [1, 3, 12]

Zero count = 2

Final = [1, 3, 12, 0, 0]

---

## 💻 Code
```java
public static int[] moveZeroesBrute(int[] arr) {
    ArrayList<Integer> list = new ArrayList<>();

    for (int num : arr) {
        if (num != 0) list.add(num);
    }

    int zeroCount = arr.length - list.size();

    while (zeroCount-- > 0) list.add(0);

    for (int i = 0; i < arr.length; i++) {
        arr[i] = list.get(i);
    }

    return arr;
}
```



# Move Zeros to End (Better Approach)

## 🧠 Intuition (Hinglish)
Better method me hum ek **temporary array** banate hain:  
- Pehle non-zero elements ko temp me daalte hain  
- Fir baaki jagah zeros bhar dete hain  
Finally temp → original me copy kar dete hain

Ye brute se better hai kyunki ek hi pass me non-zero store ho jate hain.

---

## 📝 Dry Run
Array: [0, 1, 0, 3, 12]

temp = []

- 0 → ignore  
- 1 → add → [1]  
- 0 → ignore  
- 3 → add → [1, 3]  
- 12 → add → [1, 3, 12]

Remaining space → fill with zeros  
temp = [1, 3, 12, 0, 0]

Copy back → arr = [1, 3, 12, 0, 0]

---

## 💻 Code
```java
public static void moveZeroesBetter(int[] arr) {
    int n = arr.length;
    int[] temp = new int[n];
    int index = 0;

    for (int num : arr) {
        if (num != 0) {
            temp[index++] = num;
        }
    }

    // remaining are zeros by default

    for (int i = 0; i < n; i++) {
        arr[i] = temp[i];
    }
}
```




# Move Zeros to End (Optimal Approach)

## 🧠 Intuition (Hinglish)
Optimal approach me hum **two-pointer technique** use karte hain:  
`j` pointer wo position hai jaha next non-zero element jana chahiye.  
`i` pointer normally array traverse karega.  
Jab bhi non-zero mile → usko arr[j] se swap kar do → j++

Isse:
✔ sab non-zero elements front me rearrange ho jate hain  
✔ zeros automatically end me chale jate hain  
✔ inplace O(n) solution milta hai

---

## 📝 Dry Run
Array: [0, 1, 0, 3, 12]

i=0 → 0 → skip  
i=1 → 1 → swap(arr[1], arr[0]) → [1, 0, 0, 3, 12], j=1  
i=2 → 0 → skip  
i=3 → 3 → swap(arr[3], arr[1]) → [1, 3, 0, 0, 12], j=2  
i=4 → 12 → swap(arr[4], arr[2]) → [1, 3, 12, 0, 0], j=3  

Final → [1, 3, 12, 0, 0]

---

## 💻 Code
```java
public static void moveZeroesOptimal(int[] arr) {
    int j = 0;

    for (int i = 0; i < arr.length; i++) {
        if (arr[i] != 0) {
            int temp = arr[i];
            arr[i] = arr[j];
            arr[j] = temp;
            j++;
        }
    }
}
```
