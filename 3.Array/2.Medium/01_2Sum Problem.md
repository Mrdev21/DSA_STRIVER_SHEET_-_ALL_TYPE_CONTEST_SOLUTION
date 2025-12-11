
# 2Sum Problem (Brute Approach)

## 🧠 Intuition (Hinglish)
Brute force me hum har element ko baaki sab elements ke saath check karte hain.  
Matlab har pair (i, j) try karke dekhte hain ki arr[i] + arr[j] == target hai ya nahi.  
Direct, simple approach — but time O(n²).

---

## 📝 Dry Run
Array: [2, 7, 11, 15], target = 9

Check pairs:
- i=0, j=1 → 2 + 7 = 9 → **match → answer = (0,1)**  
- Further checking ki zarurat nahi

---

## 💻 Code
```java
public static int[] twoSumBrute(int[] arr, int target) {
    int n = arr.length;
    for (int i = 0; i < n; i++) {
        for (int j = i + 1; j < n; j++) {
            if (arr[i] + arr[j] == target) {
                return new int[]{i, j};
            }
        }
    }
    return new int[]{-1, -1};
}
```


# 2Sum Problem (Better Approach)

## 🧠 Intuition (Hinglish)
Better approach me hum **HashMap** use karte hain:  
- Iterate karte time har element ka “required complement” = target - arr[i] check karte hain.  
- Agar complement map me hai → answer mil gaya.  
- Agar nahi → current element map me store karte chalo.

Isse hume O(1) me check milta hai → total O(n) time.

---

## 📝 Dry Run
Array: [2, 7, 11, 15], target = 9  
Map = {} initially

i=0 → arr[0]=2 → complement=7 → map empty → store {2:0}  
i=1 → arr[1]=7 → complement=2 → 2 map me present → **answer = (0,1)**

---

## 💻 Code
```java
public static int[] twoSumBetter(int[] arr, int target) {
    HashMap<Integer, Integer> map = new HashMap<>();

    for (int i = 0; i < arr.length; i++) {
        int complement = target - arr[i];

        if (map.containsKey(complement)) {
            return new int[]{map.get(complement), i};
        }

        map.put(arr[i], i);
    }
    return new int[]{-1, -1};
}
```




# 2Sum Problem (Optimal Approach)

## 🧠 Intuition (Hinglish)
Agar array **sorted** ho, to hum two-pointer technique use kar sakte hain:
- left = 0  
- right = n-1  
- sum = arr[left] + arr[right]  
  - sum < target → left++  
  - sum > target → right--  
  - sum == target → answer mil gaya

Ye O(n) me kaam karta hai aur extra space 0.

---

## 📝 Dry Run
Sorted Array: [2, 7, 11, 15], target = 9

left=0, right=3  
sum = 2 + 15 = 17 → >9 → right--  

left=0, right=2  
sum = 2 + 11 = 13 → >9 → right--  

left=0, right=1  
sum = 2 + 7 = 9 → **match**

Answer = (0,1)

---

## 💻 Code
```java
public static int[] twoSumOptimal(int[] arr, int target) {
    int left = 0, right = arr.length - 1;

    while (left <= right) {
        int sum = arr[left] + arr[right];

        if (sum == target) return new int[]{left, right};
        else if (sum < target) left++;
        else right--;
    }

    return new int[]{-1, -1};
}
```


