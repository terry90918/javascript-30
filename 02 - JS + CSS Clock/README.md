# 02 - JS + CSS Clock

用 JavaScript 與 CSS 製作一個實時的時鐘效果。

## Step 步驟

1. 製作時針、分針、秒針
    * 利用 CSS 來表現出時針、分針、秒針的樣式。
1. 設定定時器
    * 利用 `setInterval(setDate, 1000);` 每秒取得當前時間。
1. 利用當前時間來取得對應角度
    * 將每秒取得的時間在 `setData` 裡面取出，並計算出對應角度，再透過 `element.style.tranform` 來變更 CSS 效果，產生位移的感覺。

## Javascript

語法與備註。

### `let` & `const`

> 可以參考 [卡斯伯 - ES6 開始的新生活 let, const](https://wcc723.github.io/javascript/2017/12/20/javascript-es6-let-const/)

### `Date()`

取得時間的函數，一定要搭配 `new` 來使用 `new Date()`。

* `date.getSeconds()`：取得當前秒。
* `date.getMinutes()`：取得當前分鐘。
* `date.getHours()`：取得當前小時。

### `setInterval()`

定時器，有兩個參數 `setInterval(callback, time)`，第一個是要執行的 `function`，第二個是時間（毫秒）。

## `setData()`

1. 創建 Date 對象。
1. 獲取當前時間的小時、分鐘、秒。
1. 利用此刻的數據，計算每個指針對應的角度。
1. 模擬真實的時針，計算每一分鐘對時針的角度影響，使時針可以緩慢地移動。

```js
const hoursDegrees = (hours / 12) * 360 + (mins / 12 / 60) * 360 + 90;
```

## CSS

語法與備註。

### `transform-oragin`

變形的軸心，預設為物件的中心點，在這個範例中，設定為 100% ( right ) 可以使其從時鐘面的中心點開始旋轉。

### `transform:rotate(90deg)`

旋轉物件，數值後方要加上角度 `deg`，可超過 360 度，正值為順時針轉，負值為逆時針旋轉，`90deg` 讓時間樣式歸零。

## `transition: all .05s`

每次動畫執行到結束時間為 0.05 秒。

### `transition-timing-function: cubic-bezier(0.1, 2.7, 0.58, 1)`

設定動畫轉場所依據的**貝茲曲線**，可以透過 Chrome 的開發者工具來進行可視化調整。

## 解決難點

動動腦時間！

### 轉完一圈時，閃爍的跳針問題

當秒針旋轉一圈之後回到初始位置，開始第二圈旋轉，角度值的變化是 444° → 90°，指針會先逆時針從 444° 旋轉到 90°，會有 0.05 秒的跳轉，再繼續的順時針旋轉。解決辦法，當計算角度為 90° 時，把動畫效果關閉，避免動畫跳轉！

```js
if (secondsDegrees === 90) secondHand.style.transition = 'all 0s';
else secondHand.style.transition = 'all 0.05s';

if (minsDegrees === 90) minsHand.style.transition = 'all 0s';
else minsHand.style.transition = 'all 0.1s';
```