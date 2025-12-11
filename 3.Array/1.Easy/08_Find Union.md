# Find Union of Two Arrays (Brute Approach)

## 🧠 Intuition (Hinglish)
Brute force me hum dono arrays ke saare elements ek list me daal denge.  
Phir duplicates ko remove karne ke liye,  
hum ek element uthayenge aur check karenge ki kya woh list me pehle se exist to nahi karta.  
Agar pehle se ho → skip, nahi → add.

Ye bilkul raw comparison-based union hai.

---

## 📝 Dry Run
arr1 = [1, 2, 2, 3]  
arr2 = [2, 3, 4]

Step-by-step:
- Add 1 → union = [1]  
- Add 2 → union = [1, 2]  
- Next 2 → already exists → skip  
- Add 3 → union = [1, 2, 3]  

From arr2:
- 2 → exists → skip  
- 3 → exists → skip  
- 4 → new → union = [1, 2, 3, 4]

Final → [1, 2, 3, 4]

---

## 💻 Code
```java
public static ArrayList<Integer> unionBrute(int[] arr1, int[] arr2) {
    ArrayList<Integer> union = new ArrayList<>();

    // insert all elements, avoid duplicates manually
    for (int x : arr1) {
        if (!union.contains(x)) union.add(x);
    }
    for (int x : arr2) {
        if (!union.contains(x)) union.add(x);
    }

    return union;
}
```

# Find Union of Two Arrays (Better Approach)

## 🧠 Intuition (Hinglish)
Better method me hum **HashSet** use karenge jo automatically duplicates ignore karta hai.  
Bas dono arrays ke elements set me store karo.  
Phir set ko list me convert kar do → union mil gaya.

Simple, efficient aur clean solution.

---

## 📝 Dry Run
arr1 = [1, 2, 2, 3]  
arr2 = [2, 3, 4]

Set insertions:
- Insert 1 → {1}  
- Insert 2 → {1,2}  
- Insert 2 → duplicate skip  
- Insert 3 → {1,2,3}  
- Insert 2 → skip  
- Insert 3 → skip  
- Insert 4 → {1,2,3,4}

Final set → [1, 2, 3, 4]

---

## 💻 Code
```java
public static ArrayList<Integer> unionBetter(int[] arr1, int[] arr2) {
    HashSet<Integer> set = new HashSet<>();

    for (int x : arr1) set.add(x);
    for (int x : arr2) set.add(x);

    return new ArrayList<>(set);
}
```






# Find Union of Two Arrays (Optimal Approach)

## 🧠 Intuition (Hinglish)
Optimal approach tab possible hota hai jab **dono arrays sorted** hon.  
Hum two-pointer technique use karte hain:

- i pointer arr1 par  
- j pointer arr2 par  
- Small element ko union me add karo  
- Agar equal mile → ek hi add karo, dono pointers aage  

Isse poora union O(n + m) time me mil jata hai, bina extra overhead ke.

---

## 📝 Dry Run
arr1 = [1, 2, 2, 3]  
arr2 = [2, 3, 4]

i = 0, j = 0  
- 1 < 2 → add 1  
i=1  
- 2 == 2 → add 2  
i=2, j=1  
- 2 == 3 → add 3? NO, can't add yet → 2 < 3 → add 2 but avoid duplicate  
i=3  
- 3 == 3 → add 3  
i=4, j=2  
- arr1 over, add remaining arr2 → add 4  

Final → [1, 2, 3, 4]

---

## 💻 Code
```java
public static ArrayList<Integer> unionOptimal(int[] a, int[] b) {
    int i = 0, j = 0;
    int n = a.length, m = b.length;

    ArrayList<Integer> union = new ArrayList<>();

    while (i < n && j < m) {

        if (a[i] <= b[j]) {
            if (union.isEmpty() || union.get(union.size() - 1) != a[i]) {
                union.add(a[i]);
            }
            i++;
        } else {
            if (union.isEmpty() || union.get(union.size() - 1) != b[j]) {
                union.add(b[j]);
            }
            j++;
        }
    }

    // remaining elements of a
    while (i < n) {
        if (union.isEmpty() || union.get(union.size() - 1) != a[i]) {
            union.add(a[i]);
        }
        i++;
    }

    // remaining elements of b
    while (j < m) {
        if (union.isEmpty() || union.get(union.size() - 1) != b[j]) {
            union.add(b[j]);
        }
        j++;
    }

    return union;
}
```

