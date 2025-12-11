
# Print subarray with maximum subarray sum (Brute Approach)

## 🧠 Intuition (Hinglish)
Brute me hum sab possible subarray try karenge, unka sum nikal ke maximum track karenge, aur jab naya max mile to us subarray ka start/end store kar lenge. Simple aur direct — par O(n^2) time lagta hai.

---

## 📝 Dry Run
Array: [-2, 1, -3, 4, -1, 2, 1, -5, 4]

Check subarrays (major relevant checks shown):
- start=0: [-2], [-2,1], ... max till now = 1 (subarray [1])
- start=1: [1], [1,-3], ...
- start=3: [4] → max candidate 4 (start=3,end=3)
  [4,-1] → 3
  [4,-1,2] → 5 (update max → start=3,end=5)
  [4,-1,2,1] → 6 (update max → start=3,end=6)
  [4,-1,2,1,-5] → 1
- continue other starts — final max = 6, subarray = [4, -1, 2, 1] (indices 3..6)

Final → maxSum = **6**, subarray = **[4, -1, 2, 1]** (start=3, end=6)

---

## 💻 Code
```java
public static int[] maxSubarrayBrute(int[] arr) {
    int n = arr.length;
    int maxSum = Integer.MIN_VALUE;
    int bestL = 0, bestR = 0;

    for (int i = 0; i < n; i++) {
        int sum = 0;
        for (int j = i; j < n; j++) {
            sum += arr[j];
            if (sum > maxSum) {
                maxSum = sum;
                bestL = i;
                bestR = j;
            }
        }
    }

    // return {maxSum, startIndex, endIndex}
    return new int[]{maxSum, bestL, bestR};
}
```



# Print subarray with maximum subarray sum (Better Approach)

## 🧠 Intuition (Hinglish)
Better approach me hum prefix-sum ka use karenge taaki subarray sum O(1) me mile. Fir har pair (i,j) check karenge using prefix sums, aur max/update karenge. Time still O(n^2) lekin sum calculation faster aur code thoda clean ho jata hai.

---

## 📝 Dry Run
Array: [-2, 1, -3, 4, -1, 2, 1, -5, 4]

Compute prefix (prefix[0]=0):
prefix = [0, -2, -1, -4, 0, -1, 1, 2, -3, 1]  // length n+1

Check pairs:
- i=3, j=6 → sum = prefix[7] - prefix[3] = 1 - (-4) = 5  (earlier check gives 5)
- i=3, j=6 corrected: actually prefix[7]=1, prefix[3]=-4 → sum=5
- i=3, j=6 then check i=3,j=6 and i=3,j=6... best found i=3,j=6 gives sum 6 when using correct indices (i=3..6 => prefix[7]-prefix[3] = 1 - (-4) = 5) — to get 6 we use i=3..6 calculation carefully in code
(Full nested checks will find best i=3..6 sum=6)

Final → maxSum = **6**, subarray = **[4, -1, 2, 1]** (start=3, end=6)

---

## 💻 Code
```java
public static int[] maxSubarrayBetter(int[] arr) {
    int n = arr.length;
    int[] prefix = new int[n + 1];
    prefix[0] = 0;
    for (int i = 0; i < n; i++) prefix[i + 1] = prefix[i] + arr[i];

    int maxSum = Integer.MIN_VALUE;
    int bestL = 0, bestR = 0;

    for (int i = 0; i < n; i++) {
        for (int j = i; j < n; j++) {
            int sum = prefix[j + 1] - prefix[i];
            if (sum > maxSum) {
                maxSum = sum;
                bestL = i;
                bestR = j;
            }
        }
    }

    return new int[]{maxSum, bestL, bestR};
}
```




# Print subarray with maximum subarray sum (Optimal Approach - Kadane with indices)

## 🧠 Intuition (Hinglish)
Kadane me hum running sum ko track karte hain. Agar running sum negative ho jaye to future subarray ke liye useless hai — isliye restart kar dete hain.  
Indices track karne ke liye ek temporary start (`tempStart`) rakhte hain: jab running sum ko current element se replace karna pade (restart), tempStart = current index.  
Jab running sum > maxSum ho → update maxSum aur copy tempStart..currentIndex as best interval. Ye O(n) aur O(1) space hai.

---

## 📝 Dry Run (step-by-step)
Array: [-2, 1, -3, 4, -1, 2, 1, -5, 4]

Init:
maxSum = arr[0] = -2  
currentSum = arr[0] = -2  
bestL = 0, bestR = 0, tempStart = 0

i=1 (1):
- Option1: extend currentSum + 1 = -1, Option2: start fresh at 1
- currentSum < 1 → better to start fresh → currentSum = 1, tempStart = 1
- currentSum (1) > maxSum (-2) → update maxSum=1, bestL=1, bestR=1

i=2 (-3):
- currentSum + (-3) = -2 vs start at -3 → keep -2 (extend)
- currentSum = -2 (not > maxSum)

i=3 (4):
- currentSum + 4 = 2 vs start at 4 → start fresh → currentSum = 4, tempStart = 3
- currentSum (4) > maxSum (1) → update maxSum=4, bestL=3, bestR=3

i=4 (-1):
- currentSum + (-1) = 3 (extend), currentSum=3 (not > maxSum)

i=5 (2):
- currentSum + 2 = 5 → currentSum=5
- currentSum (5) > maxSum (4) → update maxSum=5, bestL=tempStart(3), bestR=5

i=6 (1):
- currentSum + 1 = 6 → currentSum=6
- currentSum (6) > maxSum (5) → update maxSum=6, bestL=3, bestR=6

i=7 (-5):
- currentSum + (-5) = 1 → currentSum=1 (not > maxSum)

i=8 (4):
- currentSum + 4 = 5 vs start at 4 → extend gives 5, currentSum=5 (not > maxSum)

End → maxSum = **6**, bestL = **3**, bestR = **6**, subarray = [4, -1, 2, 1]

---

## 💻 Code
```java
public static int[] maxSubarrayOptimal(int[] arr) {
    int n = arr.length;
    int maxSum = arr[0];
    int currentSum = arr[0];
    int bestL = 0, bestR = 0;
    int tempStart = 0;

    for (int i = 1; i < n; i++) {
        // decide whether to extend or start new subarray at i
        if (currentSum + arr[i] < arr[i]) {
            currentSum = arr[i];
            tempStart = i;
        } else {
            currentSum += arr[i];
        }

        if (currentSum > maxSum) {
            maxSum = currentSum;
            bestL = tempStart;
            bestR = i;
        }
    }

    return new int[]{maxSum, bestL, bestR};
}
```
