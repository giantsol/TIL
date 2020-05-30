# 123. Best Time to Buy and Sell Stock III

[Link](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii/)

```java
public int maxProfit(int[] prices) {
    int maxProfit = 0;
    for (int i = 0; i < prices.length; i++) {
        maxProfit = Math.max(maxProfit, maxProfitInRange(prices, 0, i) + maxProfitInRange(prices, i + 1, prices.length - 1));
    }

    return maxProfit;
}

private int maxProfitInRange(int[] prices, int start, int end) {
    if (start >= prices.length) {
        return 0;
    }

    int maxProfit = 0;
    int curMin = prices[start];
    for (int i = start + 1; i <= end; i++) {
        maxProfit = Math.max(maxProfit, prices[i] - curMin);
        curMin = Math.min(curMin, prices[i]);
    }

    return maxProfit;
}
```

타임 O(n^2), 스페이스 O(1)인데 discussion보니까 타임을 O(n)으로 줄일수도있네!!

내일봐야지!!