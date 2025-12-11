
# Majority Element > n/3 (Brute Approach)

## 🧠 Intuition (Hinglish)
Brute force me har element ke liye poora array scan karke uska count nikalte hain.  
Agar count > n/3 ho to usse answer list me daal dete hain.  
Har element ke liye full scan → O(n²).

---

## 📝 Dry Run
Array: [3,2,3]

Check 3 → count=2 → 2 > 3/3 (=1) → add 3  
Check 2 → count=1 → not majority  
Check 3 again → already added

Result → **[3]**

---

## 💻 Code
```java
public static List<Integer> majorityBrute(int[] arr) {
    int n = arr.length;
    List<Integer> ans = new ArrayList<>();

    for (int i = 0; i < n; i++) {
        int count = 0;
        for (int j = 0; j < n; j++) {
            if (arr[j] == arr[i]) count++;
        }

        if (count > n / 3 && !ans.contains(arr[i])) {
            ans.add(arr[i]);
        }
    }

    return ans;
}
```


# Majority Element > n/3 (Better Approach)

## 🧠 Intuition (Hinglish)
Better approach me hum **HashMap** use karte hain:  
- Frequency count nikal lo → O(n)  
- Jiska count > n/3 ho → list me daal do  
Simple aur clear — space O(n), time O(n).

---

## 📝 Dry Run
Array: [1,1,1,3,3,2,2,2]

Frequencies:
1 → 3  
2 → 3  
3 → 2  

n/3 = 8/3 = 2  
Counts > 2 → [1,2]

Result → **[1,2]**

---

## 💻 Code
```java
public static List<Integer> majorityBetter(int[] arr) {
    int n = arr.length;
    HashMap<Integer, Integer> map = new HashMap<>();

    for (int x : arr) {
        map.put(x, map.getOrDefault(x, 0) + 1);
    }

    List<Integer> ans = new ArrayList<>();
    for (int key : map.keySet()) {
        if (map.get(key) > n / 3) ans.add(key);
    }

    return ans;
}
```



# Majority Element > n/3 (Optimal Approach)

## 🧠 Intuition (Hinglish)
Classic **Boyer–Moore** majority vote algorithm ka extended form use karte hain.  
Kyunki n/3 case me **max 2 elements** hi majority ho sakte hain, isliye:

- do candidates maintain karo (candidate1, candidate2)  
- unke counts maintain karo  
- agar current number kisi candidate == ho → count++  
- agar koi count zero ho jaye → usko new candidate assign karo  
- warna dono counts -- kar do (pair removal logic)

**Second pass me verify zaroor karo** kyunki Boyer–Moore sirf candidate deta hai.

Time O(n), space O(1) — best solution.

---

## 📝 Dry Run
Array: [1,1,1,3,3,2,2,2]

**Pass 1 (candidate selection):**

Start: c1=?, c2=?, f1=0,f2=0

1 → c1=1,f1=1  
1 → f1=2  
1 → f1=3  
3 → c2=3,f2=1  
3 → f2=2  
2 → both candidates exist → f1--,f2-- → f1=2,f2=1  
2 → again → f1--,f2-- → f1=1,f2=0  
2 → f2=1,c2=2  

Final candidates: **1 and 2**

**Pass 2 (verification):**  
count of 1 = 3 (> n/3 = 2)  
count of 2 = 3 (>2)

Result → **[1,2]**

---

## 💻 Code
```java
public static List<Integer> majorityOptimal(int[] arr) {
    int n = arr.length;
    int c1 = 0, c2 = 0;
    int f1 = 0, f2 = 0;

    // 1st pass: find potential candidates
    for (int x : arr) {
        if (x == c1) {
            f1++;
        } else if (x == c2) {
            f2++;
        } else if (f1 == 0) {
            c1 = x;
            f1 = 1;
        } else if (f2 == 0) {
            c2 = x;
            f2 = 1;
        } else {
            f1--;
            f2--;
        }
    }

    // 2nd pass: verify
    f1 = 0; 
    f2 = 0;
    for (int x : arr) {
        if (x == c1) f1++;
        else if (x == c2) f2++;
    }

    List<Integer> ans = new ArrayList<>();
    if (f1 > n / 3) ans.add(c1);
    if (f2 > n / 3) ans.add(c2);

    return ans;
}
```



