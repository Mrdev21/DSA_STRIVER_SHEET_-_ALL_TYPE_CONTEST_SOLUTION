


# Maximum Subarray Sum (Brute Approach)

## 🧠 Intuition (Hinglish)
Brute force me hum har possible subarray ka sum calculate karte hain.  
Start index i se end index j tak ka sum nikalo → max update karte raho.  
Ye sabse basic approach hai, par O(n²) ya O(n³) tak ja sakta hai (depending sum calc).

---

## 📝 Dry Run
Array: [-2, 1, -3, 4, -1, 2]

Check all subarrays:

- Start 0:
  - [-2] = -2
  - [-2,1] = -1
  - [-2,1,-3] = -4
  - [-2,1,-3,4] = 0
  - ... max = 4 so far
- Start 1:
  - [1] = 1
  - [1,-3] = -2
  - [1,-3,4] = 2
  - [1,-3,4,-1] = 1
  - [1,-3,4,-1,2] = 3
- Start 3:
  - [4] = 4
  - [4,-1] = 3
  - [4,-1,2] = 5 → **max = 5**

Final maximum = **5**

---

## 💻 Code
```java
public static int maxSubarrayBrute(int[] arr) {
    int n = arr.length;
    int maxSum = Integer.MIN_VALUE;

    for (int i = 0; i < n; i++) {
        int sum = 0;
        for (int j = i; j < n; j++) {
            sum += arr[j];
            if (sum > maxSum) maxSum = sum;
        }
    }

    return maxSum;
}
```


# Maximum Subarray Sum (Better Approach)

## 🧠 Intuition (Hinglish)
Better approach me hum **prefix sum** use kar sakte hain.  
Subarray sum(i..j) = prefix[j] - prefix[i-1].  
Isse sum O(1) me nikal jata hai, but loop still O(n²) hota hai.

Brute se fast, par still quadratic.

---

## 📝 Dry Run
Array: [-2, 1, -3, 4, -1, 2]

Prefix:
p = [ -2, -1, -4, 0, -1, 1 ]

Check pairs:
- (i=3,j=5) → p[5] - p[2] = 1 - (-4) = 5 → **max = 5**
- Other pairs give smaller sums  
Final → **5**

---

## 💻 Code
```java
public static int maxSubarrayBetter(int[] arr) {
    int n = arr.length;
    int[] prefix = new int[n];
    prefix[0] = arr[0];

    for (int i = 1; i < n; i++) {
        prefix[i] = prefix[i - 1] + arr[i];
    }

    int maxSum = Integer.MIN_VALUE;

    for (int i = 0; i < n; i++) {
        for (int j = i; j < n; j++) {
            int sum = (i == 0) ? prefix[j] : prefix[j] - prefix[i - 1];
            maxSum = Math.max(maxSum, sum);
        }
    }

    return maxSum;
}
```



# Maximum Subarray Sum (Optimal Approach – Kadane's Algorithm)

## 🧠 Intuition (Hinglish)
Kadane algorithm ek simple logic follow karta hai:

**Current sum agar negative ho raha hai → isse restart kar do (0 se).  
Kyuki negative sum future subarray ko sirf bigaadta hai.**

Steps:
- currentSum = 0  
- maxSum = -∞  
- For each element:
  - currentSum += element  
  - maxSum update  
  - If currentSum < 0 → currentSum = 0  

Yeh O(n) time me best result deta hai.

---

## 📝 Dry Run
Array: [-2, 1, -3, 4, -1, 2]

currentSum = 0, maxSum = -∞

- -2 → currentSum = -2 → <0 reset → currentSum=0 → maxSum=-2  
- 1 → currentSum=1 → max=1  
- -3 → currentSum=-2 → reset → currentSum=0  
- 4 → currentSum=4 → max=4  
- -1 → currentSum=3 → max=4  
- 2 → currentSum=5 → **max=5**

Final → **5**

---

## 💻 Code
```java
public static int maxSubarrayOptimal(int[] arr) {
    int currentSum = 0;
    int maxSum = Integer.MIN_VALUE;

    for (int num : arr) {
        currentSum += num;
        maxSum = Math.max(maxSum, currentSum);

        if (currentSum < 0) {
            currentSum = 0;
        }
    }

    return maxSum;
}
```


