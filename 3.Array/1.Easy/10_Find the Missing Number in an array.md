

# Find the Missing Number (Brute Approach)

## 🧠 Intuition (Hinglish)
Brute force me hum 1 se n tak har number check kar sakte hain ki  
"kya ye number array me present hai?"  
Agar koi number missing milta hai → wahi answer.

Ye direct checking method hai, par slow (O(n²)).

---

## 📝 Dry Run
Array = [1, 2, 4, 5], n = 5

Check:
- 1 → present  
- 2 → present  
- 3 → ❌ not present → **missing = 3**  
- (no need to check further)

Answer → 3

---

## 💻 Code
```java
public static int missingBrute(int[] arr, int n) {
    for (int num = 1; num <= n; num++) {
        boolean found = false;

        for (int x : arr) {
            if (x == num) {
                found = true;
                break;
            }
        }

        if (!found) return num;
    }
    return -1;
}
```


# Find the Missing Number (Better Approach)

## 🧠 Intuition (Hinglish)
Better method me hum **HashSet** use karte hain.  
Set me sab array elements daal do →  
phir 1 to n tak check karo ki kaun sa number set me missing hai.

Ye O(n) hai, par extra space lagta hai.

---

## 📝 Dry Run
Array = [1, 2, 4, 5], n = 5

Set = {1, 2, 4, 5}

Check:
1 → in set  
2 → in set  
3 → ❌ not in set → **missing = 3**

Answer → 3

---

## 💻 Code
```java
public static int missingBetter(int[] arr, int n) {
    HashSet<Integer> set = new HashSet<>();
    
    for (int x : arr) set.add(x);

    for (int num = 1; num <= n; num++) {
        if (!set.contains(num)) return num;
    }
    return -1;
}
```






# Find the Missing Number (Optimal Approach)

## 🧠 Intuition (Hinglish)
Optimal approach me hum maths use karte hain:

Sum of 1 to n = n * (n + 1) / 2  
Array ka sum nikal lo.  
Dono ka difference = missing number.

Ye sabse fast tarika hai.

---

## 📝 Dry Run
Array = [1, 2, 4, 5], n = 5

expectedSum = 5 * 6 / 2 = 15  
actualSum = 1 + 2 + 4 + 5 = 12  

missing = expectedSum - actualSum = **3**

Answer → 3

---

## 💻 Code
```java
public static int missingOptimal(int[] arr, int n) {
    int expectedSum = n * (n + 1) / 2;
    int actualSum = 0;

    for (int x : arr) {
        actualSum += x;
    }

    return expectedSum - actualSum;
}
```





