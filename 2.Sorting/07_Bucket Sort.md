
### 🧠 Intuition
Divide input range into buckets, put items into buckets, sort each bucket (often with insertion sort), concatenate. Good for uniformly distributed data.

### ⚙️ Approach
- Create m buckets.
- Distribute elements: bucketIndex = floor(m * (x - min)/(max-min+1)).
- Sort buckets individually and merge.

### 📝 Dry Run
arr in [0,1) uniform → buckets each get few items → local sort cheap → combine.

### 💻 Code (simple)
```java
public static List<Double> bucketSort(List<Double> arr) {
    int n = arr.size();
    List<List<Double>> buckets = new ArrayList<>();
    for (int i = 0; i < n; i++) buckets.add(new ArrayList<>());
    double min = Collections.min(arr);
    double max = Collections.max(arr);
    double range = max - min + 1e-9;

    for (double v : arr) {
        int idx = (int) ((v - min) / range * n);
        if (idx == n) idx = n - 1;
        buckets.get(idx).add(v);
    }

    List<Double> result = new ArrayList<>();
    for (List<Double> b : buckets) {
        Collections.sort(b); // insertion sort better for small buckets
        result.addAll(b);
    }
    return result;
}
```

### ⏱ Complexity
- Avg: O(n + m + k) (m buckets, k cost to sort buckets)  
- Worst: O(n²) if all elements fall in same bucket  
- Stable: depends on bucket sort used  
- Use: when input uniformly distributed and range known.

---
