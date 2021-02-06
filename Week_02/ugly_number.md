# 剑指 Offer 49. 丑数

我们把只包含质因子 2、3 和 5 的数称作丑数（Ugly Number）。求按从小到大的顺序的第 n 个丑数。

*示例:*
```
输入: n = 10
输出: 12
解释: 1, 2, 3, 4, 5, 6, 8, 9, 10, 12 是前 10 个丑数。
```

*说明:  *
1. 1 是丑数。
2. n 不超过1690。

## 题解
复杂度分析：
- Time: O(n)
- Space: O(n)

第i个丑数肯定是以下3个丑数中最小的一个:
1. 第一个*2后大于第i-1个丑数的丑数
2. 第一个*3后大于第i-1个丑数的丑数
3. 第一个*5后大一第i-1个丑数的丑数

```go
func nthUglyNumber(n int) int {
    // dp represents n ugly numbers, dp[0] is first ugly number 1
    // we want to find dp[n-1]
    dp := make([]int, n)
    dp[0] = 1
    // to calculate dp[i]
    // i2 represents first index in dp, which dp[i2]*2 > dp[i-1]
    // i3 represents first index in dp, which dp[i3]*3 > dp[i-1]
    // i5 represents first index in dp, which dp[i5]*5 > dp[i-1]
    // dp[i] is the min of these three values: min(dp[i2]*2, dp[i3]*3, dp[i5]*5)
    // and if any one of these three values equals to dp[i], for
    // example dp[i2]*2 == dp[i], i2+1 will be the next first index,
    // which dp[i2+1]*2 > dp[i]
    var i2, i3, i5 int
    for i := 1; i < n; i++ {
        a, b, c := dp[i2]*2, dp[i3]*3, dp[i5]*5
        dp[i] = min(a, b, c)
        // can't use swith case or ifelse here, to prevent duplicates
        if dp[i] == a {
            i2++
        }
        if dp[i] == b {
            i3++
        }
        if dp[i] == c {
            i5++
        }
    }
    return dp[n-1]
}

func min(a, b, c int) int {
    if a <= b && a <= c {
        return a
    }
    if b <= a && b <= c {
        return b
    }
    return c
}
```
