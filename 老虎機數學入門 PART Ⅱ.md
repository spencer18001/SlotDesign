# 老虎機數學入門 PART Ⅱ

Source: https://ezslotdesign.com/slotmath2/

這篇是 PART I 的延伸。PART I 先示範一條 Line 獎與簡化 Scatter 的 RTP 計算；PART II 則加入 5 軸 Scatter 觸發免費遊戲，以及免費遊戲中的再觸發計算。

## 文章設定

這篇範例的遊戲特色：

- 5 個轉輪都放入 Scatter。
- 主遊戲中出現 3 個以上任意位置的 Scatter，觸發免費遊戲。
- 免費遊戲中出現 3 個以上任意位置的 Scatter，可以再觸發。
- 出現 3、4、5 個 Scatter 時，分別獲得 10、15、20 次 Free Spins。

```text
3 Scatter -> 10 Free Spins
4 Scatter -> 15 Free Spins
5 Scatter -> 20 Free Spins
```

## 主遊戲計算架構

主遊戲左側仍然和 PART I 一樣：

- 轉輪表
- 賠率表
- 圖案次數統計
- 一般符號 Line 獎計算

一般符號的 3、4、5 連線計算方式與 PART I 相同：

```text
出現次數 = 轉輪1可命中數 * 轉輪2可命中數 * ... * 轉輪5可命中數
出現機率 = 出現次數 / 所有可能數
RTP = 出現機率 * 賠分
```

文章範例的主遊戲 RTP 是：

```text
Main Game RTP = 65.00%
```

## Scatter 觸發率計算

PART II 的 Scatter 要列舉 3、4、5 顆 Scatter 的所有位置組合。

假設每軸共有 20 個停格，其中每軸有 3 個停格會讓畫面看到 Scatter：

```text
3S = 可看到 Scatter 的停格數 = 3
X_3S = 看不到 Scatter 的停格數 = 17
any = 該軸所有停格數 = 20
```

### 3 Scatter

3 Scatter 代表 5 軸中剛好 3 軸出現 Scatter，另外 2 軸沒有 Scatter。

單一組合例如：

```text
3S / 3S / 3S / X_3S / X_3S
```

出現次數：

```text
3 * 3 * 3 * 17 * 17 = 7,803
```

5 軸中任選 3 軸出現 Scatter，共有：

```text
C(5,3) = 10 種
```

所以：

```text
3 Scatter 觸發率 = 7,803 * 10 / 3,200,000
                 = 2.4384%
```

### 4 Scatter

4 Scatter 代表 5 軸中剛好 4 軸出現 Scatter，1 軸沒有 Scatter。

```text
單一組合出現次數 = 3 * 3 * 3 * 3 * 17 = 1,377
組合數 = C(5,4) = 5
4 Scatter 觸發率 = 1,377 * 5 / 3,200,000 = 0.2152%
```

### 5 Scatter

5 Scatter 代表 5 軸都出現 Scatter。

```text
5 Scatter 出現次數 = 3 * 3 * 3 * 3 * 3 = 243
5 Scatter 觸發率 = 243 / 3,200,000 = 0.0076%
```

因此主遊戲免費遊戲總觸發率：

```text
2.4384% + 0.2152% + 0.0076% = 2.6612%
```

## 為什麼要分 3、4、5 Scatter

3、4、5 Scatter 都會觸發免費遊戲，但不是同一種事件，因為給的免費遊戲次數不同。

| Scatter 數 | 觸發類型 | 免費遊戲次數 |
|---:|---|---:|
| 3 | 3 Scatter Trigger | 10 |
| 4 | 4 Scatter Trigger | 15 |
| 5 | 5 Scatter Trigger | 20 |

所以不能只算「有沒有觸發免費遊戲」，而是要分別算：

```text
3 Scatter 的觸發率
4 Scatter 的觸發率
5 Scatter 的觸發率
```

後面算 Free Game RTP 時，這三種會分別乘上不同的免費遊戲次數。

## 免費遊戲單 Spin RTP

免費遊戲的單 Spin RTP 計算方式和主遊戲相同，但免費遊戲轉輪表有差異：

- 轉輪上放入更多 Wild。
- 第 5 軸少了最後位置 20 的符號 9。
- 因此一般 Line 獎 RTP 變高。
- Scatter 出現頻率也和主遊戲不同。

文章算出的單一把免費遊戲 RTP 是：

```text
Free Spin RTP = 102.28%
```

注意：這裡的 102.28% 是「一個 Free Spin 本身」的 RTP，還沒有把再觸發後增加的額外 spins 算進去。

## 免費遊戲再觸發率

免費遊戲中同樣可能出現 3、4、5 Scatter，並且再給 10、15、20 次 Free Spins。

文章中的免費遊戲再觸發率是：

```text
P3 = 2.5064%
P4 = 0.2238%
P5 = 0.0080%
```

其中：

```text
P3 = 每一個 Free Spin 中再觸發 3 Scatter 的機率
P4 = 每一個 Free Spin 中再觸發 4 Scatter 的機率
P5 = 每一個 Free Spin 中再觸發 5 Scatter 的機率
```

對應增加的免費遊戲次數：

```text
S3 = 10
S4 = 15
S5 = 20
```

## 含再觸發後的平均 Spin 數：等比級數推導

這篇最重要的理論是：如何計算「1 個 Free Spin 含再觸發後，平均等於幾個 Free Spins」。

先定義：

```text
P3 = 一個 Free Spin 觸發 3 Scatter 的機率
P4 = 一個 Free Spin 觸發 4 Scatter 的機率
P5 = 一個 Free Spin 觸發 5 Scatter 的機率

S3 = 3 Scatter 給的 Free Spins = 10
S4 = 4 Scatter 給的 Free Spins = 15
S5 = 5 Scatter 給的 Free Spins = 20
```

每一個 Free Spin 平均會「再生出」多少個新的 Free Spins？

```text
r = P3*S3 + P4*S4 + P5*S5
```

`r` 可以理解成：

```text
每消耗 1 個 Free Spin，平均會因為 retrigger 產生 r 個新的 Free Spins。
```

如果一開始只有 1 個 Free Spin，則平均總 spins 可以分代來看。

第 0 代是原本那 1 個 Free Spin：

```text
1
```

第 1 代是原本那 1 個 Free Spin 平均再觸發出的新 spins：

```text
r
```

第 2 代是第 1 代那些 Free Spins 又繼續再觸發出的 spins：

```text
r^2
```

第 3 代：

```text
r^3
```

依此類推，1 個 Free Spin 含所有 retrigger 後的平均總 spins 數為：

```text
X = 1 + r + r^2 + r^3 + ...
```

這是一個無窮等比級數。只要：

```text
0 <= r < 1
```

級數就會收斂，而且：

```text
1 + r + r^2 + r^3 + ... = 1 / (1 - r)
```

所以：

```text
X = 1 / (1 - r)
```

把 `r` 代回去：

```text
X = 1 / (1 - (P3*S3 + P4*S4 + P5*S5))
```

也就是：

```text
X = 1 / (1 - P3*S3 - P4*S4 - P5*S5)
```

用文章數字來看：

```text
P3 = 2.5064% = 0.025064
P4 = 0.2238% = 0.002238
P5 = 0.0080% = 0.000080

S3 = 10
S4 = 15
S5 = 20
```

先算平均每個 Free Spin 會再生出的 spins：

```text
r = P3*S3 + P4*S4 + P5*S5

r ≈ 0.025064*10
  + 0.002238*15
  + 0.000080*20

r ≈ 0.25064 + 0.03357 + 0.00160
r ≈ 0.28581
```

所以：

```text
X = 1 / (1 - 0.28581)
  = 1 / 0.71419
  ≈ 1.40018608
```

意思是：

```text
1 個 Free Spin，考慮所有可能再觸發後，平均最後會變成 1.40018608 個 Free Spins。
```

## 觸發 3、4、5 Scatter 後的平均總 Free Spins

因為 3、4、5 Scatter 初始給的 Free Spins 不同，所以要分別乘上 X：

```text
3 Scatter: 10 * 1.40018608 = 14.0019
4 Scatter: 15 * 1.40018608 = 21.0028
5 Scatter: 20 * 1.40018608 = 28.0037
```

| 觸發類型 | 初始 Free Spins | 含再觸發後平均總次數 |
|---:|---:|---:|
| 3 Scatter | 10 | 14.0019 |
| 4 Scatter | 15 | 21.0028 |
| 5 Scatter | 20 | 28.0037 |

## Free Game RTP 計算

Free Game RTP 的計算公式是：

```text
Free Game RTP = 主遊戲觸發率 * 單一 Free Spin RTP * 含再觸發後平均總 Free Spins
```

分開看：

```text
3 Scatter 貢獻 = 2.4384% * 102.28% * 14.0019 = 34.92%
4 Scatter 貢獻 = 0.2152% * 102.28% * 21.0028 = 4.62%
5 Scatter 貢獻 = 0.0076% * 102.28% * 28.0037 = 0.22%
```

加總：

```text
Free Game RTP = 34.92% + 4.62% + 0.22% = 39.76%
```

## 總 RTP

最後，總 RTP 是主遊戲 RTP 加上免費遊戲 RTP：

```text
Total RTP = Main Game RTP + Free Game RTP
          = 65.00% + 39.76%
          = 104.76%
```

## 小結

PART II 的核心可以整理成三層：

1. 主遊戲：計算一般 Line 獎與 Scatter 觸發率。
2. 免費遊戲：計算一個 Free Spin 本身的 RTP。
3. 再觸發：用等比級數算出含再觸發後的平均 Free Spins 數。

最重要的直覺是：

```text
原本的 Free Spin
+ 第一輪 retrigger 生出的 Free Spins
+ 第二輪 retrigger 又生出的 Free Spins
+ 第三輪 retrigger 又生出的 Free Spins
+ ...
```

寫成級數就是：

```text
X = 1 + r + r^2 + r^3 + ... = 1 / (1 - r)
```

其中：

```text
r = P3*S3 + P4*S4 + P5*S5
```

因此：

```text
X = 1 / (1 - P3*S3 - P4*S4 - P5*S5)
```
