# 老虎機數學入門 PART Ⅴ

來源：https://ezslotdesign.com/slotmath5/

PART V 的主題是：常見 Feature Game / Free Game 觸發方式，以及不同觸發規則在數學計算上的差異。

前幾篇比較偏向「某一種獎項怎麼算 RTP」，這一篇比較像整理：

```text
FG 觸發條件有很多種，
不同條件會決定你要怎麼拆事件、怎麼算觸發率。
```

---

## 1. 全畫面 Scatter 個數觸發

最常見的免費遊戲觸發方式：

```text
整個畫面出現 N 個以上 Scatter -> 觸發 FG
```

常見例子：

```text
3S -> 觸發
4S -> 觸發，可能給更多 free spins
5S -> 觸發，可能給更多 free spins 或更高獎勵
```

計算重點：

```text
P(trigger)
= P(3S) + P(4S) + P(5S)
```

如果不同 Scatter 個數給不同獎勵，就不能只算總觸發率，而要分開算：

```text
期望獎勵
= P(3S) × reward(3S)
 + P(4S) × reward(4S)
 + P(5S) × reward(5S)
```

### 一輪最多一個 Scatter

如果設計限制為：

```text
每一軸畫面最多只會出現 1 個 Scatter
```

那 3x5 畫面最多就是 5 個 Scatter。

這種設計通常會讓 Scatter 在輪帶上保持距離，避免同一軸畫面內同時看到兩個 Scatter。

計算時可以把每一軸視為：

```text
該軸有沒有出現 Scatter
```

再組合 5 軸，計算剛好 3、4、5 軸出現 Scatter 的機率。

### 一輪可以多個 Scatter

如果 Scatter 可以堆疊，同一軸畫面可能出現 0、1、2、3 個 Scatter。

這時不能只看「有沒有 Scatter」，而要看：

```text
每一軸畫面中出現幾個 Scatter
```

再把五軸的 Scatter 個數加總，計算：

```text
總 Scatter 數 = 6
總 Scatter 數 = 7
總 Scatter 數 = 8
...
```

這類計算會比「一軸最多一個 Scatter」更細，因為同一軸可能貢獻多顆 Scatter。

### Excel 通用列法

如果要在 Excel 裡計算「Scatter 可以堆疊」的情況，可以分成兩層表：

```text
1. 每軸 S 個數分布表
2. 五軸 S 總數 partition 表
```

第一層先對每一軸逐 stop 判斷該停輪畫面會出現幾顆 S。

以 3 格高、輪帶長度 20 為例，每個 stop 都檢查：

```text
stop i
stop i+1
stop i+2
```

然後記錄這個 3 格視窗中有幾顆 S：

```text
stop | 視窗1 | 視窗2 | 視窗3 | S個數
1    | ...   | ...   | ...   | 2
2    | ...   | ...   | ...   | 1
3    | ...   | ...   | ...   | 0
...
```

接著統計該軸的分布：

```excel
=COUNTIF(S個數欄,0)
=COUNTIF(S個數欄,1)
=COUNTIF(S個數欄,2)
=COUNTIF(S個數欄,3)
```

得到：

```text
第1軸：0S 有幾個 stop、1S 有幾個 stop、2S 有幾個 stop、3S 有幾個 stop
第2軸：0S 有幾個 stop、1S 有幾個 stop、2S 有幾個 stop、3S 有幾個 stop
...
```

第二層再列五軸組合，計算總 Scatter 數。

例如要算全畫面剛好 3 顆 Scatter，就列出所有滿足：

```text
R1_S + R2_S + R3_S + R4_S + R5_S = 3
```

的 partition。

表格可以長這樣：

```text
R1 | R2 | R3 | R4 | R5 | Total S | 出現次數
0  | 0  | 0  | 0  | 3  | 3       | R1_0S × R2_0S × R3_0S × R4_0S × R5_3S
0  | 0  | 0  | 1  | 2  | 3       | R1_0S × R2_0S × R3_0S × R4_1S × R5_2S
0  | 0  | 1  | 1  | 1  | 3       | R1_0S × R2_0S × R3_1S × R4_1S × R5_1S
...
```

最後把 `Total S = 3` 的所有 partition 出現次數加總，就是 3S 的出現次數。

同理：

```text
Total S = 4 -> 4S 出現次數
Total S = 5 -> 5S 出現次數
Total S >= 3 -> 觸發次數
```

整理成一句話：

```text
先做「每軸 S 個數分布表」，
再做「五軸 S 總數 partition 表」。
```

---

## 2. 由左至右連續軸 Scatter 觸發

另一種觸發方式是：

```text
Scatter 必須由左到右連續出現
```

例如：

```text
第1、2、3軸出現 Scatter -> 3 連觸發
第1、2、3、4軸出現 Scatter -> 4 連觸發
第1、2、3、4、5軸出現 Scatter -> 5 連觸發
```

計算重點：

```text
只看左到右連續，不是全畫面任意位置湊滿個數。
```

所以這種算法比較像 Line / Way 的「連續軸」概念。

若一軸最多一個 Scatter，可以直接拆成：

```text
3S 連續
4S 連續
5S 連續
```

若一軸可以多個 Scatter，則每一軸仍要先算該軸畫面有幾種 Scatter 出現方式，再組合連續軸。

---

## 3. 只出現在特定滾輪的 Scatter

有些遊戲不讓 Scatter 出現在所有軸，而是只出現在特定幾軸。

例如：

```text
Scatter 只出現在第2、3、4軸
且三軸都出現才觸發
```

這時事件空間變小，計算也比較單純：

```text
P(trigger)
= P(第2軸出現 S)
 × P(第3軸出現 S)
 × P(第4軸出現 S)
```

如果各軸輪帶不同，就要分別使用各軸自己的機率。

---

## 4. Line Trigger

有些 Feature Game 不是看整個畫面，而是看得分線。

### 一條線上有一定個數 Scatter

例如：

```text
某條 payline 上出現 3 個 Scatter -> 觸發
```

這和「全畫面 Scatter」不同，因為 Scatter 必須落在指定 payline 的位置上。

計算提醒：

```text
不能把每軸畫面高度 3 直接乘進去。
```

因為 Line Trigger 只看線上的格子，不看整個 3 格視窗。

如果有多條線，還要考慮：

```text
每條線的觸發率
總共有幾條線
不同線之間是否可能同時觸發
```

### 一條線上由左至右連續 Scatter

這類規則要求：

```text
Scatter 必須沿著 payline 由左至右連續出現
```

計算方式和一般 Line Pay 類似，只是符號換成 Scatter / Bonus symbol。

---

## 5. 多種 Scatter / Bonus Symbol 混合觸發

有些遊戲會使用多種特殊符號一起判斷。

例如：

```text
2 個 Bonus Symbol
+ 任一種 Scatter
-> 觸發不同獎勵
```

計算重點是先定義清楚：

```text
哪些符號可以混合計數？
哪些符號要分開計數？
不同組合給的獎勵是否相同？
```

如果不同組合給不同獎勵，要分開算期望值：

```text
期望值
= P(組合A) × reward(A)
 + P(組合B) × reward(B)
 + P(組合C) × reward(C)
```

---

## 6. 隨機觸發

有些 Feature Game 不由畫面符號觸發，而是後台直接設定機率。

例如：

```text
每一把 spin 有 1% 機率觸發 FG
```

這種情況不用從輪帶計算 Scatter 組合。

觸發率就是：

```text
P(trigger) = 1%
```

期望觸發間隔：

```text
平均每 1 / P(trigger) 次 spin 觸發一次
```

例如：

```text
P(trigger) = 1%
平均 100 spin 觸發一次
```

---

## 7. 蒐集型觸發

蒐集型 Feature 通常是：

```text
畫面上出現某種寶物 / 能量
累積到指定數量後觸發
```

例如：

```text
需要收集 1000 個寶物
每把平均出現 3 個
```

則平均觸發間隔約為：

```text
1000 / 3 = 333.33 spins
```

換成每把平均觸發率，可以粗略理解為：

```text
3 / 1000 = 0.3%
```

但這裡要注意：蒐集型通常不是單把獨立事件，而是跨 spin 累積狀態，所以如果要做完整模型，需要追蹤 meter 狀態。

---

## 8. 假裝蒐集，實際隨機觸發

有些遊戲表演上看起來像蒐集型：

```text
畫面出現寶物
能量條慢慢累積
滿了觸發
```

但實際數學上可能仍是後台隨機觸發。

這種情況要分清楚：

```text
表演邏輯 ≠ 數學觸發邏輯
```

如果本質是隨機觸發，計算上仍然用：

```text
P(trigger) = 後台設定機率
```

---

## 總結

PART V 的重點不是某一條固定公式，而是判斷 Feature Game 的觸發類型。

計算前要先問：

```text
1. 觸發是看全畫面，還是看 payline？
2. Scatter 是看總個數，還是看由左至右連續軸？
3. 每一軸最多一個 Scatter，還是可以多個 Scatter？
4. Scatter 是否只出現在特定滾輪？
5. 是否有多種 Bonus / Scatter 符號混合判斷？
6. 觸發是由畫面符號決定，還是後台隨機決定？
7. 是否有跨 spin 累積狀態？
```

整理成一句話：

```text
Feature Game 觸發率的計算，本質上就是先定義事件，再把所有會觸發的事件 partition 拆乾淨。
```
