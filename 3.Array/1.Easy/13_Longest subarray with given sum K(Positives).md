# Longest subarray with given sum K (Brute Approach)

## 🧠 Intuition (Hinglish)
Brute force me hum sab possible subarrays generate karenge (start index i se end index j tak) aur unka sum calculate karenge.
Jab sum == K mile to us subarray ki length note karenge aur maximum lenge.
Simple hai, par time O(n^2) lagta hai kyunki har start ke liye end move karte hain.

---

## 📝 Dry Run
Array: [1, 2, 3, 2, 5], K = 5

Check subarrays:
- start 0: [1]=1, [1,2]=3, [1,2,3]=6 (>5) → stop inner? (but brute checks all)
- start 1: [2]=2, [2,3]=5 → match → length = 2 (max=2)
- start 2: [3]=3, [3,2]=5 → match → length = 2 (max=2)
- start 3: [2]=2, [2,5]=7
- start 4: [5]=5 → match → length = 1 (max still 2)

Final longest length → **2**

---

## 💻 Code
```java
public static int longestSubarrayBrute(int[] arr, int K) {
    int n = arr.length;
    int maxLen = 0;

    for (int i = 0; i < n; i++) {
        int sum = 0;
        for (int j = i; j < n; j++) {
            sum += arr[j];
            if (sum == K) {
                int len = j - i + 1;
                if (len > maxLen) maxLen = len;
            }
            // (for positives we could break when sum > K to micro-optimize,
            // but brute keeps full inner loop to show pure O(n^2) behavior)
        }
    }

    return maxLen;
}
```


# Longest subarray with given sum K (Better Approach)

## 🧠 Intuition (Hinglish)
Better approach me hum prefix-sum + HashMap use kar sakte hain:
- prefixSum[i] = sum of elements from 0..i
- Agar at index i prefixSum - K ka earlier index j mila (means prefixSum[i] - prefixSum[j] = K),
  to subarray (j+1 .. i) ka sum K hai.
- HashMap me pehli baar aane wala prefix sum ka index store karo (earliest index) taaki longest length mile.
Ye O(n) time deta hai lekin O(n) extra space lagta hai.

Note: Yeh approach negatives ke liye bhi kaam karega; positives ke case me sliding-window (optimal) aur bhi simple hai.

---

## 📝 Dry Run
Array: [1, 2, 3, 2, 5], K = 5

i=0: prefix=1 → store map {1:0}  
i=1: prefix=3 → store {1:0, 3:1}  
i=2: prefix=6 → prefix-5=1 found at index 0 → subarray (1..2) length = 2 → max=2; store {6:2}  
i=3: prefix=8 → prefix-5=3 found at index 1 → subarray (2..3) length = 2 → max=2; store {8:3}  
i=4: prefix=13 → prefix-5=8 found at index 3 → subarray (4..4) length = 1 → max=2

Final longest length → **2**

---

## 💻 Code
```java
public static int longestSubarrayBetter(int[] arr, int K) {
    int n = arr.length;
    HashMap<Integer, Integer> firstIndex = new HashMap<>();
    int prefix = 0;
    int maxLen = 0;

    for (int i = 0; i < n; i++) {
        prefix += arr[i];

        if (prefix == K) {
            maxLen = Math.max(maxLen, i + 1); // subarray from 0..i
        }

        // if prefix-K seen before, possible subarray (index+1 .. i)
        if (firstIndex.containsKey(prefix - K)) {
            int prevIndex = firstIndex.get(prefix - K);
            maxLen = Math.max(maxLen, i - prevIndex);
        }

        // store earliest index for this prefix sum
        if (!firstIndex.containsKey(prefix)) {
            firstIndex.put(prefix, i);
        }
    }

    return maxLen;
}
```









# Longest subarray with given sum K (Optimal Approach)

## 🧠 Intuition (Hinglish)
Since array me **sirf positive numbers** hain, sliding-window (two pointers) sabse simple aur optimal hai:
- Maintain window [start..end], aur current window sum.
- Agar sum < K → expand window by moving `end` (add arr[end]).
- Agar sum > K → shrink window by moving `start` (subtract arr[start]).
- Agar sum == K → update max length, phir continue expand/shrink as needed.
Yeh O(n) time aur O(1) extra space deta hai — best for positives.

---

## 📝 Dry Run
Array: [1, 2, 3, 2, 5], K = 5

start=0, end=0, sum=1  
sum < 5 → end=1, sum=3  
sum < 5 → end=2, sum=6  
sum > 5 → shrink start: sum=6-1=5, start=1  
sum == 5 → window [1..2] length = 2 (max=2)  
continue: end=3, sum=5+2=7  
sum > 5 → shrink start: sum=7-2=5, start=2  
sum == 5 → window [2..3] length = 2 (max=2)  
end=4, sum=5+5=10  
sum > 5 → shrink start: sum=10-3=7, start=3  
sum > 5 → shrink start: sum=7-2=5, start=4  
sum == 5 → window [4..4] length = 1 (max still 2)

Final longest length → **2**

---

## 💻 Code
```java
public static int longestSubarrayOptimal(int[] arr, int K) {
    int n = arr.length;
    int start = 0;
    int sum = 0;
    int maxLen = 0;

    for (int end = 0; end < n; end++) {
        sum += arr[end];

        // shrink window while sum > K and start <= end
        while (start <= end && sum > K) {
            sum -= arr[start];
            start++;
        }

        if (sum == K) {
            maxLen = Math.max(maxLen, end - start + 1);
        }
    }

    return maxLen;
}
```
