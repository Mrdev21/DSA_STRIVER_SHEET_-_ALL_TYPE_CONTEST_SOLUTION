

# Maximum Consecutive Ones (Brute Approach)

## 🧠 Intuition (Hinglish)
Brute force me hum har index se start karke aage jaake count karenge ki kitni consecutive `1`s milti hain.  
Har start position ke liye ek inner loop chalega jo consecutive ones count karega — aur sabme se maximum choose karenge.  
Simple hai lekin time zyada lagta hai (O(n²)).

---

## 📝 Dry Run
Array: [1, 1, 0, 1, 1, 1]

Start at i=0:
- j=0 → 1 → count=1
- j=1 → 1 → count=2
- j=2 → 0 → stop → max = 2

Start at i=1:
- j=1 → 1 → count=1
- j=2 → 0 → stop → max = 2

Start at i=2:
- j=2 → 0 → count=0 → max = 2

Start at i=3:
- j=3 → 1 → count=1
- j=4 → 1 → count=2
- j=5 → 1 → count=3 → max = 3

Final → **3**

---

## 💻 Code
```java
public static int maxConsecutiveOnesBrute(int[] arr) {
    int n = arr.length;
    int maxCount = 0;

    for (int i = 0; i < n; i++) {
        int count = 0;
        for (int j = i; j < n; j++) {
            if (arr[j] == 1) count++;
            else break;
        }
        if (count > maxCount) maxCount = count;
    }

    return maxCount;
}
```





# Maximum Consecutive Ones (Better Approach)

## 🧠 Intuition (Hinglish)
Better approach me hum single pass use karenge:  
Current run ka counter rakho — jab `1` mile increment karo, jab `0` mile to counter reset karo.  
Har step pe max update kar lo. Yeh O(n) time aur O(1) space deta hai.

---

## 📝 Dry Run
Array: [1, 1, 0, 1, 1, 1]

i=0 → arr[0]=1 → curr=1 → max=1  
i=1 → arr[1]=1 → curr=2 → max=2  
i=2 → arr[2]=0 → curr=0 → max=2  
i=3 → arr[3]=1 → curr=1 → max=2  
i=4 → arr[4]=1 → curr=2 → max=2  
i=5 → arr[5]=1 → curr=3 → max=3

Final → **3**

---

## 💻 Code
```java
public static int maxConsecutiveOnesBetter(int[] arr) {
    int maxCount = 0;
    int curr = 0;

    for (int num : arr) {
        if (num == 1) {
            curr++;
            if (curr > maxCount) maxCount = curr;
        } else {
            curr = 0;
        }
    }

    return maxCount;
}
```





# Maximum Consecutive Ones (Optimal Approach)

## 🧠 Intuition (Hinglish)
Better aur optimal dono same single-pass logic use karte hain.  
Optimal me code ko thoda aur concise rakhenge — single loop, ek variable current run, ek variable max.  
Edge-cases: empty array → return 0. Ye O(n) time, O(1) space hai aur best possible.

---

## 📝 Dry Run
Array: [0, 0, 0] → curr always 0 → max 0 → result 0  
Array: [1, 1, 1] → curr grows to 3 → result 3  
Array: [] → result 0

Example: [1, 1, 0, 1, 1, 1]
Same as better → Final → **3**

---

## 💻 Code
```java
public static int maxConsecutiveOnesOptimal(int[] arr) {
    int maxCount = 0;
    int curr = 0;

    for (int i = 0; i < arr.length; i++) {
        if (arr[i] == 1) {
            curr++;
            maxCount = Math.max(maxCount, curr);
        } else {
            curr = 0;
        }
    }

    return maxCount;
}
```




