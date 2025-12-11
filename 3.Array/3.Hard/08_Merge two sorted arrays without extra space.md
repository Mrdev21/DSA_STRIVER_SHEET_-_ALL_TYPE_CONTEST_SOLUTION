
# Merge Two Sorted Arrays Without Extra Space (Brute Approach)

## 🧠 Intuition (Hinglish)
Brute force idea:  
- Dono arrays ko ek hi large array me combine kar do (extra space use karke).  
- Us combined array ko sort kar do.  
- Fir wapas split kar do pehle wale arr1 me first n elements aur baaki arr2 me.

Lekin question specifically **"without extra space"** bolta hai → brute bas conceptual understanding ke liye.

Time: O((n+m) log(n+m))  
Space: O(n+m)

---

## 📝 Dry Run
arr1 = [1, 4, 8, 10]  
arr2 = [2, 3, 9]

Combined → [1,4,8,10,2,3,9]  
Sorted → [1,2,3,4,8,9,10]

Split:
arr1 = [1,2,3,4]  
arr2 = [8,9,10]

---

## 💻 Code
```java
public static void mergeBrute(int[] a, int[] b) {
    int n = a.length, m = b.length;
    int[] temp = new int[n + m];
    int idx = 0;

    for (int x : a) temp[idx++] = x;
    for (int x : b) temp[idx++] = x;

    Arrays.sort(temp);

    for (int i = 0; i < n; i++) a[i] = temp[i];
    for (int j = 0; j < m; j++) b[j] = temp[n + j];
}
```



# Merge Two Sorted Arrays Without Extra Space (Better Approach)

## 🧠 Intuition (Hinglish)
Better technique:  
- arr1 ke har element ko arr2 ke first element se compare karo.  
- Agar arr1[i] > arr2[0], to swap arr1[i] ↔ arr2[0].  
- Ab arr2[0] ko sahi jagah par insert karne ke liye **arr2 ko sort / shift** karna hoga.

Isse extra array nahi use hota, sirf arr2 ke andar shifting hoti hai.

Time: O(n * m) (worst case)  
Space: O(1)

---

## 📝 Dry Run
arr1 = [1, 4, 8, 10]  
arr2 = [2, 3, 9]

i=0: 1 < 2 → ok  
i=1: 4 > 2 → swap → arr1=[1,2,8,10], arr2=[4,3,9]  
arr2 ko sort/shift → [3,4,9]

i=2: 8 > 3 → swap → arr1=[1,2,3,10], arr2=[8,4,9]  
shift arr2 → [4,8,9]

i=3: 10 > 4 → swap → arr1=[1,2,3,4], arr2=[10,8,9]  
shift arr2 → [8,9,10]

Final:
arr1 = [1,2,3,4]  
arr2 = [8,9,10]

---

## 💻 Code
```java
public static void mergeBetter(int[] a, int[] b) {
    int n = a.length, m = b.length;

    for (int i = 0; i < n; i++) {
        if (a[i] > b[0]) {
            // swap a[i] with smallest element of b
            int temp = a[i];
            a[i] = b[0];
            b[0] = temp;

            // place b[0] at correct place in b (insertion sort step)
            int first = b[0];
            int k = 1;
            while (k < m && b[k] < first) {
                b[k - 1] = b[k];
                k++;
            }
            b[k - 1] = first;
        }
    }
}
```


# Merge Two Sorted Arrays Without Extra Space (Optimal Approach — Gap Method)

## 🧠 Intuition (Hinglish)
Optimal method = **Shell Sort / Gap Method**  
Steps:
1. Combine length = n+m  
2. Initial gap = ceil((n+m)/2)  
3. Compare elements that are `gap` apart:  
   - case1: both in arr1  
   - case2: arr1 vs arr2  
   - case3: both in arr2  
4. If arr[x] > arr[y], swap.  
5. Gap ko half karte jao until gap = 1 → 0.

This is the cleanest O((n+m) log(n+m)) with **O(1) space**.

---

## 📝 Dry Run
arr1 = [1,4,8,10]  
arr2 = [2,3,9]

n=4,m=3 → total=7  
gap = ceil(7/2)=4  

Gap = 4  
- compare positions with difference 4  
  (0 in arr1) vs (0 in arr2) → (1 vs 2) ok  
  (1 vs 1) → (4 vs 3) swap  
arr1=[1,3,8,10] arr2=[2,4,9]

Gap = ceil(4/2)=2  
Compare every 2-gap pair → swaps happen until sorted

Gap = 1  
Final after gap=1 pass:
arr1 = [1,2,3,4]  
arr2 = [8,9,10]

---

## 💻 Code
```java
public static void mergeOptimal(int[] a, int[] b) {
    int n = a.length, m = b.length;
    int gap = (n + m + 1) / 2;

    while (gap > 0) {
        int i = 0, j = gap;

        while (j < n + m) {
            // element1: location based on i (a or b)
            int val1 = (i < n) ? a[i] : b[i - n];
            // element2: location based on j
            int val2 = (j < n) ? a[j] : b[j - n];

            if (val1 > val2) {
                if (i < n && j < n) {
                    // both in arr1
                    int temp = a[i];
                    a[i] = a[j];
                    a[j] = temp;
                } else if (i < n && j >= n) {
                    // i in arr1, j in arr2
                    int temp = a[i];
                    a[i] = b[j - n];
                    b[j - n] = temp;
                } else {
                    // both in arr2
                    int temp = b[i - n];
                    b[i - n] = b[j - n];
                    b[j - n] = temp;
                }
            }

            i++;
            j++;
        }

        if (gap == 1) break;
        gap = (gap + 1) / 2;
    }
}
```
