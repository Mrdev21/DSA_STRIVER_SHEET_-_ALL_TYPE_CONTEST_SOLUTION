

# Find the Smallest Divisor (Brute Approach)

## 🧠 Intuition (Hinglish)
Brute force:  
Divisor `d` ki value 1 se lekar max(nums) tak try karte hain.

Har divisor ke liye:
- compute: sum = Σ ceil(nums[i] / d)
- agar sum <= threshold → valid divisor
- smallest valid divisor return karo

Bahut slow if max(nums) bada ho.

---

## 📝 Dry Run
nums = [1,2,5,9], threshold = 6

Try d = 1 → sum = 1+2+5+9 = 17 >6  
d = 2 → sum = 1+1+3+5 = 10 >6  
d = 3 → sum = 1+1+2+3 = 7 >6  
d = 4 → sum = 1+1+2+3 = 7 >6  
d = 5 → sum = 1+1+1+2 = 5 ≤6 → answer = **5**

---

## 💻 Code
```java
public static int smallestDivisorBrute(int[] nums, int threshold) {
    int max = 0;
    for (int x : nums) max = Math.max(max, x);

    for (int d = 1; d <= max; d++) {
        if (compute(nums, d) <= threshold)
            return d;
    }
    return max;
}

private static int compute(int[] nums, int d) {
    int sum = 0;
    for (int x : nums) {
        sum += (x + d - 1) / d;  // ceil(x/d)
    }
    return sum;
}
```



# Find the Smallest Divisor (Better Approach — Early Stop)

## 🧠 Intuition (Hinglish)
Brute hi hai but improvement:  
Jaisi hi sum > threshold ho, loop break kar do.  
Worst-case abhi bhi O(max * n) hai but average me better.

---

## 📝 Dry Run
nums = [1,2,5,9], threshold = 6  
d=1 → sum quickly >6, break early  
d=2 → sum >6 early, break  
...  
d=5 → sum <=6 → answer = 5

---

## 💻 Code
```java
public static int smallestDivisorBetter(int[] nums, int threshold) {
    int max = 0;
    for (int x : nums) max = Math.max(max, x);

    for (int d = 1; d <= max; d++) {
        if (computeEarly(nums, d, threshold) <= threshold)
            return d;
    }
    return max;
}

private static int computeEarly(int[] nums, int d, int threshold) {
    int sum = 0;
    for (int x : nums) {
        sum += (x + d - 1) / d;
        if (sum > threshold) return sum;  // early break
    }
    return sum;
}
```




# Find the Smallest Divisor (Optimal Approach — Binary Search on Answer)

## 🧠 Intuition (Hinglish)
Yeh **Binary Search on Answer** ka classic problem hai.

Key point:  
Agar divisor `d` valid hai (sum <= threshold), toh **bigger divisor bhi valid hoga**  
→ monotonic condition  
→ binary search possible.

Range:
- low = 1  
- high = max(nums)

Binary search:
- mid divisor test karo  
- sum = Σ ceil(nums[i] / mid)  
- agar sum <= threshold → mid valid → high = mid - 1  
- else low = mid + 1  

Final answer = low

Time = O(n log maxVal)

---

## 📝 Dry Run
nums = [1,2,5,9], threshold = 6  
max=9  
low=1, high=9

mid=5 → sum=1+1+1+2=5 ≤6 → valid → high=4  
mid=2 → sum=1+1+3+5=10 >6 → low=3  
mid=3 → sum=1+1+2+3=7 >6 → low=4  
mid=4 → sum=1+1+2+3=7 >6 → low=5  

loop ends → low=5  
**answer = 5**

---

## 💻 Code
```java
public static int smallestDivisorOptimal(int[] nums, int threshold) {
    int max = 0;
    for (int x : nums) max = Math.max(max, x);

    int low = 1, high = max, ans = max;

    while (low <= high) {
        int mid = low + (high - low) / 2;

        if (compute(nums, mid) <= threshold) {
            ans = mid;
            high = mid - 1;   // try smaller divisor
        } else {
            low = mid + 1;    // divisor too small
        }
    }
    return ans;
}

private static int compute(int[] nums, int d) {
    int sum = 0;
    for (int x : nums) {
        sum += (x + d - 1) / d;  // ceil(x/d)
    }
    return sum;
}
```




### ⭐ Summary Table

|Approach|Time|Space|Notes|
|---|---|---|---|
|Brute|O(maxVal * n)|O(1)|Try all divisors|
|Better|O(maxVal * n)|O(1)|Early break|
|**Optimal**|**O(n log maxVal)**|O(1)|Binary Search on Answer|


