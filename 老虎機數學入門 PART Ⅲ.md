# 老虎機數學入門 PART Ⅲ

來源：https://ezslotdesign.com/slotmath3/

這篇主要討論的是：**當第一軸也出現 Wild 時，Line Game 的 RTP 要怎麼計算。**

在 PART I、PART II 的範例中，Wild 通常不出現在第一軸，所以計算時可以把 Wild 當成「幫助一般圖案連線」的替代符號。但 PART III 的差異是：

```text
第一軸也有 Wild
```

這會讓同一條線同時可能符合：

```text
一般圖案連線
Wild 自己的連線
```

例如：

```text
W W W K K
```

它可以被視為：

```text
K 5 連線
W 3 連線
```

如果賠率表是：

```text
K 5 連線 = 100
W 3 連線 = 150
```

那這條線就應該給 Wild 3 連線的 150 分，而不是 K 5 連線的 100 分。

所以 PART III 的重點是：

```text
同一個 stop 組合可能同時符合多種獎項時，要判斷應該給哪一個獎。
```

通常規則會是：

```text
同一條 line 取最高派彩
```

---

## 核心概念：把中獎空間切成 Partition

這篇文章的計算方法，本質上是在做 partition enumeration。

也就是把所有會中獎的 stop 組合，拆成很多個互斥的條件區塊：

```text
partition 1
partition 2
partition 3
...
```

每個 partition 都有明確條件，例如：

```text
K K K K K
K K K K X_WK
W W W K K
W W W W K
W W W W W
```

每一列分別計算：

```text
出現次數
= 轉輪1符合條件的個數
 × 轉輪2符合條件的個數
 × 轉輪3符合條件的個數
 × 轉輪4符合條件的個數
 × 轉輪5符合條件的個數
```

接著：

```text
出現機率 = 出現次數 / 所有可能數
```

最後：

```text
RTP 貢獻 = 出現機率 × 該 partition 的派彩
```

把所有 partition 的 RTP 貢獻加總，就是總 RTP。

---

## 為什麼第一軸有 Wild 會變複雜

如果 Wild 不在第一軸，Wild 通常只能幫一般圖案延伸連線。

但如果第一軸有 Wild，則可能出現：

```text
W W W ...
```

這時 Wild 自己也可能形成連線。

所以對某一個 partition，不能只問：

```text
這是 K 幾連線？
```

還要問：

```text
這是 Wild 幾連線？
```

然後比較：

```text
該 partition 派彩
= MAX(一般圖案派彩, Wild 前導連線派彩)
```

例如：

```text
W W W K K
```

同時符合：

```text
K 5 連線 = 100
W 3 連線 = 150
```

所以派彩取：

```text
MAX(100, 150) = 150
```

---

## 文章拆成三種計算

文章把計算拆成三塊：

```text
1. 不包含 Wild 的一般圖案連線
2. 包含 Wild 替代的一般圖案連線
3. 純 Wild 連線
```

這不是唯一的拆法，但很適合 Excel 手算，因為每一塊的邏輯都比較清楚。

---

## 1. 不包含 Wild 的一般圖案連線

這一段只計算純圖案本身，不把 Wild 加進去。

例如 K：

```text
K K K K K
K K K K X_WK
K K K X_WK any
```

其中：

```text
X_WK = 不是 W，也不是 K 的 stop
```

這樣可以避免把含 Wild 的組合重複算進來。

例如：

```text
K K K K K
```

出現次數：

```text
K1 × K2 × K3 × K4 × K5
```

RTP：

```text
出現次數 / 所有可能數 × K 5 連線賠率
```

---

## 2. 包含 Wild 替代的一般圖案連線

這一段處理 Wild 代替一般圖案的情況。

以 K 為例，要列舉有 1、2、3、4 個 Wild 的各種位置：

```text
W K K K K
K W K K K
K K W K K
K K K W K
K K K K W

W W K K K
W K W K K
W K K W K
...
```

每一列都是一個 partition。

對每一列要計算：

```text
1. 出現次數
2. 一般圖案連線賠率
3. Wild 前導連線賠率
4. 最終取較高者
```

例如：

```text
W W W K K
```

出現次數：

```text
W1 × W2 × W3 × K4 × K5
```

派彩判斷：

```text
K 5 連線 = 100
W 3 連線 = 150
```

所以：

```text
該列派彩 = 150
```

---

## 前導 Wild 數

Wild 自己的連線必須從第一軸開始連續出現，所以重點不是整條線有幾個 Wild，而是：

```text
從第一軸開始，連續有幾個 W
```

例如：

```text
W W W K K  -> 前導 W 數 = 3
W K W K K  -> 前導 W 數 = 1
K W W K K  -> 前導 W 數 = 0
W W W W K  -> 前導 W 數 = 4
```

Excel 可以用公式計算前導 W 數。

穩定版公式：

```excel
=IF(D4<>"W",0,IF(E4<>"W",1,IF(F4<>"W",2,IF(G4<>"W",3,IF(H4<>"W",4,5)))))
```

意思是：

```text
如果第 1 軸不是 W，前導 W 數 = 0
否則看第 2 軸
如果第 2 軸不是 W，前導 W 數 = 1
否則繼續往後檢查
```

---

## 3. 純 Wild 連線

最後一段是純 Wild 的連線：

```text
W W W W W
W W W W S
W W W S any
```

這裡的重點是：如果只要算 Wild 自己的 4 連線或 3 連線，下一軸必須是 Wild 不能替代的符號。

文章用 `S` 表示這種 Wild 不能替代的符號，通常會是 Scatter。

例如：

```text
W W W W S
```

代表 Wild 4 連線。

如果第五軸不是 `S`，而是一般圖案，可能會同時構成一般圖案的 5 連線，計算上就會落到「含 Wild 替代的一般圖案連線」那一段處理。

---

## 可以只拆成兩個 Partition 嗎？

理論上也可以拆成：

```text
1. 第一軸是 W
2. 第一軸不是 W
```

因為 Wild 自己的連線一定需要第一軸是 W。

但這種拆法比較像程式枚舉，Excel 會比較難看。

文章拆成三段：

```text
不含 Wild
含 Wild 替代
純 Wild
```

優點是：

```text
1. 比較容易檢查有沒有漏算
2. 比較不容易重複計算
3. 每一段的公式邏輯比較單純
4. 適合手算與 Excel 展開
```

所以文章的拆法不是唯一正確，而是比較乾淨、比較適合教學與驗算。

---

## 總結

PART III 的核心不是新的機率公式，而是更仔細地切 partition。

當第一軸也有 Wild 時：

```text
同一個 stop 組合可能同時符合一般圖案獎與 Wild 獎
```

所以每個 partition 都要判斷：

```text
一般圖案賠率是多少？
Wild 前導連線賠率是多少？
最後應該取哪一個？
```

核心公式可以記成：

```text
RTP
= Σ 每個 partition 的機率 × 該 partition 的最高派彩
```

而文章的三段式計算：

```text
不含 Wild 的一般圖案
+ 含 Wild 替代的一般圖案
+ 純 Wild
```

就是一種適合 Excel 的 partition enumeration 方法。

---

## Partition 要怎麼切才不重複計獎

PART III 的核心問題是：

```text
同一條線上，Wild 可以同時被看成一般圖案的替代，也可能自己形成 Wild 連線。
```

例如：

```text
W W W K K
```

這一條線同時可以被解讀成：

```text
K 5 連線
W 3 連線
```

真實遊戲通常不是兩個都給，而是同一條 line 只給最高獎項：

```text
pay = MAX(K 5 連線賠率, W 3 連線賠率)
```

所以 partition 的切法要讓每一種 stop 組合只落在一個計算區塊，或至少在同一列裡用 `MAX()` 決定最後派彩，不能讓同一個 Wild 連線在 K/Q/J/10/9 分頁各算一次。

### 1. 沒有 Wild 的一般圖案區塊

這一區只放完全沒有 Wild 參與的普通連線。

以 K 為例：

```text
K K K K K
K K K K X_WK
K K K X_WK any
```

其中：

```text
X_WK = 該軸不是 W，也不是 K 的 stop 數
```

這樣可以保證 4 連線、3 連線不會被更長的 K/WK 連線吃掉。

### 2. 有 Wild 替代的一般圖案區塊

這一區放「winning span 裡有 Wild，也至少有一個真正的目標圖案」的組合。

以 K 為例，5 連線會列：

```text
W K K K K
K W K K K
K K W K K
K K K W K
K K K K W

W W K K K
W K W K K
W K K W K
...
```

但這一區不要放純 Wild leading 的列，例如：

```text
W W W X_WK any
W W W W X_WK
W W W W W
```

原因是這些列在 winning span 內沒有真正的 K，它們本質上是 Wild 連線，應該交給「純 Wild 區塊」處理。若每個一般圖案分頁都放一次，就會讓同一個 `W W W ...` 停輪型態在 K/Q/J/10/9 都被算到。

換句話說，一般圖案 mixed partition 的規則是：

```text
1. winning span 內至少有一個 W
2. winning span 內至少有一個真正的目標圖案
3. 若前導 Wild 數 >= 3，該列派彩用：
   MAX(一般圖案連線賠率, Wild 連線賠率)
```

例如：

```text
W W W K K
```

這列可以放在 K 的 mixed 區塊，因為 winning span 內有真正的 K。

派彩判斷：

```text
K 5 = 100
W 3 = 150
pay = MAX(100, 150) = 150
```

### 3. 純 Wild 區塊

純 Wild 區塊專門處理沒有一般圖案可歸屬的前導 Wild 連線：

```text
W W W W W
W W W W S
W W W S any
```

這裡的 `S` 只是文章用來表示「斷掉 Wild 連線的非 Wild 符號」，不是一定指 Scatter。重點是第 4 軸或第 5 軸不是 W，讓 Wild 連線停在 3 連或 4 連。

### 實作上的檢查原則

如果用 Excel 手列 partition，可以用這個順序檢查：

```text
1. 先列沒有 Wild 的一般圖案連線。
2. 再列有 Wild 替代、且 winning span 內至少有一個真正目標圖案的列。
3. 前導 Wild >= 3 的 mixed 列，用 MAX(一般圖案賠率, Wild 賠率)。
4. 純 Wild leading 的列只放在純 Wild 區塊，不放進 K/Q/J/10/9 的 mixed 區塊。
```

這樣切的好處是：文章式 partition 仍然可以用乘法快速算出每列出現次數，同時避免把同一個純 Wild 連線在多個一般圖案分頁重複計獎。
