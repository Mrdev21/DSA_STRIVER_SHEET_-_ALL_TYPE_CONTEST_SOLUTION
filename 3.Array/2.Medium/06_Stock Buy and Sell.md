

# Stock Buy and Sell (Brute Approach)

## 🧠 Intuition (Hinglish)
Brute force me hum har possible buy day aur har possible sell day try karenge (buy index < sell index).  
Har pair ke liye profit = price[sell] - price[buy] nikal ke maximum profit track karenge.  
Simple hai — sab pairs check karo — lekin O(n^2) time lagta hai.

---

## 📝 Dry Run
Prices: [7, 1, 5, 3, 6, 4]

Check all pairs:
- buy@0(7): sell@1..5 → profits = [-6, -2, -4, -1, -3] → best so far = -1 (but we keep max ≥ 0 eventually)
- buy@1(1): sell@2..5 → profits = [4, 2, 5, 3] → best so far = 5
- buy@2(5): sell@3..5 → profits = [-2, 1, -1] → best still = 5
- buy@3(3): sell@4..5 → profits = [3, 1] → best still = 5
- buy@4(6): sell@5 → profit = -2

Final max profit → **5** (buy at 1, sell at 6)

---

## 💻 Code
```java
public static int maxProfitBrute(int[] prices) {
    if (prices == null || prices.length < 2) return 0;
    int n = prices.length;
    int maxProfit = 0;

    for (int i = 0; i < n - 1; i++) {
        for (int j = i + 1; j < n; j++) {
            int profit = prices[j] - prices[i];
            if (profit > maxProfit) maxProfit = profit;
        }
    }

    return maxProfit;
}
```





# Stock Buy and Sell (Better Approach)

## 🧠 Intuition (Hinglish)
Better approach me hum prefix-min idea use karenge:  
Har index i ko sell day maan ke usse pehle ka minimum price (best buy) pata kar lo → profit = prices[i] - minSoFar.  
Isko precompute karne ke liye ek array bana sakte ho jisme leftMin[i] store karega 0..i ke minimum price — phir har i par profit check karlo.  
Time O(n), space O(n) (better than brute in time, but uses extra space).

---

## 📝 Dry Run
Prices: [7, 1, 5, 3, 6, 4]

leftMin array (min up to that index):
- leftMin[0]=7
- leftMin[1]=min(7,1)=1
- leftMin[2]=1
- leftMin[3]=1
- leftMin[4]=1
- leftMin[5]=1

Compute profits using leftMin:
- sell@1 (1): profit = 1-7 = -6 → skip
- sell@2 (5): profit = 5-1 = 4 → max=4
- sell@3 (3): profit = 3-1 = 2 → max still 4
- sell@4 (6): profit = 6-1 = 5 → max=5
- sell@5 (4): profit = 4-1 = 3 → max still 5

Final → **5**

---

## 💻 Code
```java
public static int maxProfitBetter(int[] prices) {
    if (prices == null || prices.length < 2) return 0;
    int n = prices.length;
    int[] leftMin = new int[n];
    leftMin[0] = prices[0];

    for (int i = 1; i < n; i++) {
        leftMin[i] = Math.min(leftMin[i - 1], prices[i]);
    }

    int maxProfit = 0;
    for (int i = 1; i < n; i++) {
        int profit = prices[i] - leftMin[i - 1]; // buy must be before sell
        if (profit > maxProfit) maxProfit = profit;
    }

    return maxProfit;
}
```



# Stock Buy and Sell (Optimal Approach)

## 🧠 Intuition (Hinglish)
Optimal single-pass approach me hum ek variable `minPriceSoFar` track karenge (best buy seen till now) aur ek `maxProfit`.  
Har day ko sell consider karo: profit = prices[i] - minPriceSoFar → update maxProfit.  
Saath hi `minPriceSoFar` ko bhi update karte jao.  
Ye O(n) time aur O(1) extra space deta hai — clean aur best.

---

## 📝 Dry Run
Prices: [7, 1, 5, 3, 6, 4]

Init: minPriceSoFar = +∞, maxProfit = 0

i=0 price=7 → minPriceSoFar=7, profit=0 → maxProfit=0  
i=1 price=1 → minPriceSoFar=1, profit=0 → maxProfit=0  
i=2 price=5 → profit=5-1=4 → maxProfit=4  
i=3 price=3 → profit=3-1=2 → maxProfit still 4  
i=4 price=6 → profit=6-1=5 → maxProfit=5  
i=5 price=4 → profit=4-1=3 → maxProfit still 5

Final → **5**

---

## 💻 Code
```java
public static int maxProfitOptimal(int[] prices) {
    if (prices == null || prices.length < 2) return 0;

    int minPriceSoFar = Integer.MAX_VALUE;
    int maxProfit = 0;

    for (int price : prices) {
        if (price < minPriceSoFar) {
            minPriceSoFar = price;
        } else {
            int profit = price - minPriceSoFar;
            if (profit > maxProfit) maxProfit = profit;
        }
    }

    return maxProfit;
}
```



