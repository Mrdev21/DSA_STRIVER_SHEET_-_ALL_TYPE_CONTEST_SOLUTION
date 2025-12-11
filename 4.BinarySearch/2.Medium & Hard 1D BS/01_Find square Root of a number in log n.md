

# Find Square Root of a Number (Brute Approach)

## 🧠 Intuition (Hinglish)
Brute me hum simply 1 se n tak check karte jaate hain:
- i*i <= n → continue  
- jaisi hi i*i > n hota hai → answer = i-1  

Slow but correct.

---

## 📝 Dry Run
n = 27  

i=1 → 1  
i=2 → 4  
i=3 → 9  
i=4 → 16  
i=5 → 25  
i=6 → 36 > 27 → stop → **ans = 5**

---

## 💻 Code
```java
public static int sqrtBrute(int n) {
    int i = 1;
    while (i * 1L * i <= n) i++;
    return i - 1;
}
```


# Find Square Root (Better Approach — Linear but up to sqrt(n))

## 🧠 Intuition (Hinglish)
n tak loop karne ki jagah hum sqrt(n) tak hi iterate karte hain.  
i*i n se aage jaate hi ruk jao.  
Still O(√n), better than brute.

---

## 📝 Dry Run
n = 50

i=1,4,9,16,25,36,49  
i=8 → 64 > 50 → stop → **ans = 7**

---

## 💻 Code
```java
public static int sqrtBetter(int n) {
    int i = 0;
    while ((long)i * i <= n) i++;
    return i - 1;
}
```


# Find Square Root (Optimal Approach — Binary Search, O(log n))

## 🧠 Intuition (Hinglish)
Optimal method = **Binary Search on answer**.  
Answer lies between **0 and n** (or 0 to n/2 for large n).  
Binary search mid = possible sqrt.  
- agar mid*mid == n → return mid  
- agar mid*mid < n → mid is valid candidate → low = mid + 1  
- agar mid*mid > n → high = mid - 1  

Loop ke baad **high** = floor(sqrt(n)).

Time: **O(log n)**  
Space: O(1)

---

## 📝 Dry Run
n = 27  
low=0, high=27  

mid=13 → 169 > 27 → high=12  
mid=6 → 36 > 27 → high=5  
mid=2 → 4 < 27 → ans=2, low=3  
mid=4 → 16 < 27 → ans=4, low=5  
mid=5 → 25 < 27 → ans=5, low=6  

low > high → stop  
**ans = 5**

---

## 💻 Code
```java
public static int sqrtOptimal(int n) {
    int low = 0, high = n;
    int ans = 0;

    while (low <= high) {
        int mid = low + (high - low) / 2;
        long sq = (long) mid * mid;

        if (sq == n) return mid;
        if (sq < n) {
            ans = mid;        // mid is possible floor value
            low = mid + 1;
        } else {
            high = mid - 1;
        }
    }
    return ans;
}
```
