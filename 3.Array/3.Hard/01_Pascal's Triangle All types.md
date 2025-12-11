# Pascal's Triangle – Print full triangle (Brute & Optimal)

## 🧠 Intuition (Hinglish)
Pascal triangle ka rule simple hai:
- Har row ka first & last element = 1  
- Beech ke elements previous row ke upar wale 2 elements ka sum hote hain  
  → `triangle[i][j] = triangle[i-1][j-1] + triangle[i-1][j]`

Is basis par hum pura triangle build kar sakte hain.

---

## 📝 Dry Run (N = 5)
Row0:        [1]  
Row1:       [1 1]  
Row2:      [1 2 1]  
Row3:     [1 3 3 1]  
Row4:    [1 4 6 4 1]

---

## 💻 Code (Build full Pascal Triangle)
```java
public static List<List<Integer>> generate(int n) {
    List<List<Integer>> res = new ArrayList<>();
    for (int i = 0; i < n; i++) {
        List<Integer> row = new ArrayList<>();
        for (int j = 0; j <= i; j++) {
            if (j == 0 || j == i) row.add(1);
            else row.add(res.get(i - 1).get(j - 1) + res.get(i - 1).get(j));
        }
        res.add(row);
    }
    return res;
}
```

# Pascal's Triangle – Nth Row Only (Better & Optimal)

## 🧠 Intuition (Hinglish)
Row generate karne ka simple rule:
- Nth row ka jth element = nCj  
- Direct combinations formula se row O(n) me generate ho sakta hai  
- Efficient method:  
  ```
  row[0] = 1  
  next = row[j] = row[j-1] * (n-j)/j
  ```

---

## 📝 Dry Run (N = 4)
Row:  
j=0 → 1  
j=1 → 1 * (4-1)/1 = 4  
j=2 → 4 * (4-2)/2 = 6  
j=3 → 6 * (4-3)/3 = 4  
j=4 → 1  

Row = **[1,4,6,4,1]**

---

## 💻 Optimal Code (O(n) time, O(1) extra space)
```java
public static List<Integer> getRow(int n) {
    List<Integer> row = new ArrayList<>();
    long val = 1;
    row.add(1);

    for (int j = 1; j <= n; j++) {
        val = val * (n - j + 1) / j;
        row.add((int) val);
    }
    return row;
}
```


# nCr – Pascal Element (Optimal)

## 🧠 Intuition (Hinglish)
Pascal ke kisi bhi element ka direct mathematical formula hota hai:
```
nCr = n! / (r! (n-r)!)
```

But factorial overflow aur O(n) multiplication avoid karne ke liye:
```
ans = 1
ans = ans * (n-i) / (i+1)  for i = 0 to r-1
```

Ye O(r) me answer deta hai, bahut optimal.

---

## 📝 Dry Run
n = 5, r = 2  
5C2 = 10

Step:
ans = 1  
i=0 → ans = ans * 5 / 1 = 5  
i=1 → ans = ans * 4 / 2 = 10  

---

## 💻 Code
```java
public static long nCr(int n, int r) {
    if (r > n) return 0;
    long res = 1;

    for (int i = 0; i < r; i++) {
        res = res * (n - i) / (i + 1);
    }

    return res;
}
```


# Pascal’s Triangle – Using nCr Formula Only (Optimal)

## 🧠 Intuition (Hinglish)
Har row ke har element ko direct formula se calculate kar sakte ho:
```
element = nCj
```
Isse:
- Time = O(n²)  
- Space = O(1) (triangle store karne ke alawa)  
- No need for previous rows

---

## 📝 Dry Run (Row 3)
3C0 = 1  
3C1 = 3  
3C2 = 3  
3C3 = 1  
→ Row = [1,3,3,1]

---

## 💻 Code
```java
public static List<List<Integer>> generateOptimal(int n) {
    List<List<Integer>> res = new ArrayList<>();

    for (int row = 0; row < n; row++) {
        List<Integer> curRow = new ArrayList<>();
        long val = 1;
        curRow.add(1);

        for (int j = 1; j <= row; j++) {
            val = val * (row - j + 1) / j;
            curRow.add((int) val);
        }

        res.add(curRow);
    }

    return res;
}
```









