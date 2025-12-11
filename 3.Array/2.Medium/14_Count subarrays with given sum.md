# Count subarrays with given sum (Brute Approach)

## 🧠 Intuition (Hinglish)
Brute force me hum har possible subarray consider karenge (start i se end j tak), uska sum nikal kar check karenge agar sum == K to count++.  
Simple aur direct — lekin time O(n^2) (aur O(1) extra space).

---

## 📝 Dry Run
Array: [1, 1, 1], K = 2

Check all subarrays:
- start 0: [1]=1, [1,1]=2 → count=1, [1,1,1]=3
- start 1: [1]=1, [1,1]=2 → count=2
- start 2: [1]=1

Final count → **2**

---

## 💻 Code
```java
public static int countSubarraysBrute(int[] arr, int K) {
    int n = arr.length;
    int count = 0;

    for (int i = 0; i < n; i++) {
        int sum = 0;
        for (int j = i; j < n; j++) {
            sum += arr[j];
            if (sum == K) count++;
        }
    }

    return count;
}
```



# Count subarrays with given sum (Better Approach — for positive numbers)

## 🧠 Intuition (Hinglish)
Agar array me **sirf positive numbers** ho to sliding-window (two pointers) use kar sakte ho:  
Maintain window sum; agar sum < K → expand end; agar sum > K → shrink start; agar sum == K → count++ and then move start (ya end) appropriately.  
Yeh O(n) time aur O(1) space deta hai — lekin **negative numbers** hone par yeh fail karega.

---

## 📝 Dry Run
Array: [1, 2, 1, 1, 3], K = 3

start=0,end=0,sum=1  
sum<3 → end=1,sum=3 → sum==K → count=1 → move start (or end) to continue  
move start: start=1,sum=2  
sum<3 → end=2,sum=3 → count=2 → start=2,sum=1  
end=3,sum=2 → end=4,sum=5 → sum>3 → shrink start: start=3,sum=4 → shrink: start=4,sum=3 → count=3

Final count → **3**

---

## 💻 Code
```java
// Works only when all numbers are non-negative (preferably positive)
public static int countSubarraysBetterPositives(int[] arr, int K) {
    int n = arr.length;
    int start = 0;
    int sum = 0;
    int count = 0;

    for (int end = 0; end < n; end++) {
        sum += arr[end];

        while (start <= end && sum > K) {
            sum -= arr[start];
            start++;
        }

        if (sum == K) {
            count++;
            // after counting, move start to look for next window
            sum -= arr[start];
            start++;
        }
    }

    return count;
}
```






# Count subarrays with given sum (Optimal Approach — Prefix-sum + HashMap)

## 🧠 Intuition (Hinglish)
General (positives + negatives) case ke liye best trick **prefix-sum + HashMap** hai:
- runningPrefix = sum of elements from 0..i  
- We need number of earlier indices j such that runningPrefix - prefix[j] = K → prefix[j] = runningPrefix - K  
- HashMap store karega prefixSum → frequency (kitni baar woh prefix aaya)  
- Har index pe map.get(runningPrefix - K) jitni baar mile, utna contribution add karo  
- Don't forget to count the case runningPrefix == K (ya map initialised with map.put(0,1))

Time O(n), Space O(n). Works with negatives too.

---

## 📝 Dry Run
Array: [1, -1, 1, 1], K = 1

Iterate:
- init map {0:1}, running=0, count=0
- i=0: running=1 → running-K = 0 → map[0]=1 → count=1 ; map.put(1,1)
- i=1: running=0 → running-K = -1 → map[-1]=0 → count=1 ; map.put(0,2)
- i=2: running=1 → running-K = 0 → map[0]=2 → count +=2 → count=3 ; map.put(1,2)
- i=3: running=2 → running-K = 1 → map[1]=2 → count +=2 → count=5 ; map.put(2,1)

Final count → **5**  (matches full enumeration)

---

## 💻 Code
```java
public static int countSubarraysOptimal(int[] arr, int K) {
    int count = 0;
    int running = 0;
    HashMap<Integer, Integer> freq = new HashMap<>();
    // prefix sum 0 occurs once (empty prefix)
    freq.put(0, 1);

    for (int x : arr) {
        running += x;

        // number of prefixes with sum = running - K
        int need = running - K;
        if (freq.containsKey(need)) {
            count += freq.get(need);
        }

        // record current running prefix
        freq.put(running, freq.getOrDefault(running, 0) + 1);
    }

    return count;
}
```




