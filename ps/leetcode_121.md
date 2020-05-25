# 121. Best Time to Buy and Sell Stock

[Link](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)

```java
public int maxProfit(int[] prices) {
    if (prices.length < 2) {
        return 0;
    }

    int curMinPrice = prices[0];
    int curMaxProfit = 0;

    for (int i = 1; i < prices.length; i++) {
        curMaxProfit = Math.max(curMaxProfit, prices[i] - curMinPrice);
        curMinPrice = Math.min(curMinPrice, prices[i]);
    }

    return curMaxProfit;
}
```

타임 O(n), 스페이스 O(1)!!
