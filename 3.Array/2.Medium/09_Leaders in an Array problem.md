
# Leaders in an Array (Brute Approach)

## 🧠 Intuition (Hinglish)
Brute force me hum har element ko uthayenge aur uske right wale sab elements ko check karenge.  
Agar koi bhi element uske right me usse bada na mile to woh leader hai.  
Simple hai — lekin har element ke liye poora right side scan karna padta hai → O(n^2).

---

## 📝 Dry Run
Array: [16, 17, 4, 3, 5, 2]

Check each element:
- 16 → right side = [17,4,3,5,2] → 17 > 16 → 16 **not** leader
- 17 → right side = [4,3,5,2] → none > 17 → **leader**
- 4 → right side = [3,5,2] → 5 > 4 → not leader
- 3 → right side = [5,2] → 5 > 3 → not leader
- 5 → right side = [2] → none > 5 → **leader**
- 2 → right side = [] → last element always leader → **leader**

Leaders (in array order) → **[17, 5, 2]**

---

## 💻 Code
```java
public static List<Integer> leadersBrute(int[] arr) {
    List<Integer> leaders = new ArrayList<>();
    int n = arr.length;

    for (int i = 0; i < n; i++) {
        boolean isLeader = true;
        for (int j = i + 1; j < n; j++) {
            if (arr[j] > arr[i]) {
                isLeader = false;
                break;
            }
        }
        if (isLeader) leaders.add(arr[i]);
    }

    return leaders;
}
```



# Leaders in an Array (Better Approach)

## 🧠 Intuition (Hinglish)
Better approach me hum suffix maximum array bana lenge:  
suffixMax[i] = maximum of elements from i..n-1.  
Fir har index i check karenge ki arr[i] >= suffixMax[i+1] (ya arr[i] > all to its right).  
Time O(n), extra space O(n) for suffix array.

---

## 📝 Dry Run
Array: [16, 17, 4, 3, 5, 2]  
Compute suffixMax (max from i..end):
index:   0   1  2  3  4  5
arr:    16  17  4  3  5  2
suffix: 17  17  5  5  5  2   (suffixMax[i] = max(arr[i], suffixMax[i+1]))

Now check leaders: compare arr[i] with suffixMax[i+1]
- i=0: arr[0]=16, suffixMax[1]=17 → 16 not leader
- i=1: arr[1]=17, suffixMax[2]=5  → 17 leader
- i=2: arr[2]=4,  suffixMax[3]=5  → not leader
- i=3: arr[3]=3,  suffixMax[4]=5  → not leader
- i=4: arr[4]=5,  suffixMax[5]=2  → 5 leader
- i=5: arr[5]=2,  suffixMax[6]= -inf (treat as -∞) → 2 leader

Leaders → **[17, 5, 2]**

---

## 💻 Code
```java
public static List<Integer> leadersBetter(int[] arr) {
    int n = arr.length;
    List<Integer> leaders = new ArrayList<>();
    if (n == 0) return leaders;

    int[] suffixMax = new int[n];
    suffixMax[n - 1] = arr[n - 1];
    for (int i = n - 2; i >= 0; i--) {
        suffixMax[i] = Math.max(arr[i], suffixMax[i + 1]);
    }

    for (int i = 0; i < n; i++) {
        // if it's the last element or greater than all to its right
        if (i == n - 1 || arr[i] > suffixMax[i + 1]) {
            leaders.add(arr[i]);
        }
    }

    return leaders;
}
```


# Leaders in an Array (Optimal Approach)

## 🧠 Intuition (Hinglish)
Optimal approach single-pass from right se hai:
Right se traverse karte hue `maxSoFar` track karo (max of elements seen to the right).  
Jo element `arr[i]` `>= maxSoFar` ho (ya strict `>` depending definition) use leader samjho aur `maxSoFar` update karo.  
Ye O(n) time aur O(1) extra space (output list ko count nahi kar rahe) deta hai.  
Note: Right-to-left collect karne se leaders reverse order me milte hain—agar original order chahiye to final me reverse kar do.

---

## 📝 Dry Run
Array: [16, 17, 4, 3, 5, 2]

Traverse from right:
- start maxSoFar = -∞
- i=5: arr[5]=2 → 2 > -∞ → leader add 2; maxSoFar=2
- i=4: arr[4]=5 → 5 > 2  → leader add 5; maxSoFar=5
- i=3: arr[3]=3 → 3 > 5? no
- i=2: arr[2]=4 → 4 > 5? no
- i=1: arr[1]=17 → 17 > 5 → leader add 17; maxSoFar=17
- i=0: arr[0]=16 → 16 > 17? no

Collected (right-to-left) → [2, 5, 17]  
Reverse to restore array order → **[17, 5, 2]**

---

## 💻 Code
```java
public static List<Integer> leadersOptimal(int[] arr) {
    List<Integer> leadersRev = new ArrayList<>();
    int n = arr.length;
    if (n == 0) return leadersRev;

    int maxSoFar = Integer.MIN_VALUE;
    for (int i = n - 1; i >= 0; i--) {
        if (arr[i] > maxSoFar) {
            leadersRev.add(arr[i]);      // collected in reverse order
            maxSoFar = arr[i];
        }
    }

    // reverse to get leaders in left-to-right order
    Collections.reverse(leadersRev);
    return leadersRev;
}
```





