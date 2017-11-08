# Chap 01. 簡介 jQuery

> jQuery 到底是什麼? 為何要用 jQuery?  
> Unobtrusive JavaScript 策略  
> 選擇正確的 jQuery 版本  
> jQuery 的基礎元素與觀念

"只有兩種語言：一種被抱怨，而另一種沒人使用。" 設計並實作 C++ 的 Bjarne Stroustrup 一語道破 JavaScript 的觀點。JavaScript以及其他語言(特別是 PHP)，被唱衰 "爛" 語言。接著，神奇的事發生了。感謝 Ajax 的興起，幾個像是 Prototype、Moo Tools 與 jQuery 程式庫的釋出，以及高互動性的新 web 應用程式：單頁應用程式 的出現，開發者開始了解 JavaScript 的潛力。JavaScript 在今日也是最普及的語言之一了，這得感謝將 JavaScript 作為伺服器端語言的 Node.js 平台，以及 PhoneGap 這個可用來建立多重裝置應用程式的框架。

jQuery 是自由 (MIT)、受歡迎的 JavaScript 程式庫，自 John Resig 在 2006 年建立，被設計來簡化 HTML 的客戶端指令稿。正如 jQuery 網站所提到的：

> jQuery 是小而快且特性豐富的 JavaScript 程式庫。跨瀏覽器且易於使用的 API，讓 HTML 文件走訪與處理、事件處理、動畫與 Ajax 更為簡單。結合多樣性與擴充性，jQuery 改變了數百萬人撰寫 JavaScript 的方式。

2015 年四月統計，前一百萬個網站中，jQuery 的使用達 63%，競爭者 Moo Tools 僅 3%，Prototype 只有 2.5%。

本書涵蓋：選擇器與走訪文件物件模型到進階的東西，plugin、改進程式碼效能與測試。

## 寫得更少，做得更多

為了要與 IE 6 以上版本相容，可能會有像是以下的程式碼實作：

```javascript
for (var i = 0; i < elements.length; i++) {
  if (elements[i].type === 'radio' && elements[i].name === 'some-radio-group' && elements[i].checked) {
    checkedValue = elements[i].value;
    break;
  }
}
```

使用 jQuery：

```javascript
var checkedValue = jQuery('input:radio[name="some-radio-group"]:checked').val();
```

jQuery會讓你的網頁活靈活現，這裡表達重點是，這程式庫是怎麼把許多行程式碼變成一行。

上述 jQuery 程式碼這麼短是來自選擇器的威力，不僅如此，它最強的地方之一是運用各種選擇器來取得元素的能力，而且不用擔心跨瀏覽器相容性的問題。

當你進行選取時，主要會用到兩個東西：方法與選取器。目前最新版本的主流瀏覽器都支援原生的元素選取方法，像是 document.querySelector() 與 document.querySelectorAll()。它們能讓我們運用各種選擇器，而不是使用 ID 或類別 (class) 來選取。

此外，現在瀏覽器也多支援新的 CSS3 選擇器。但如果要支援舊瀏覽器，jQuery就還是得使用的主要原因之一了。

<small style='border: 1px solid #ccc; padding: 10px;'>注意>>> 現今什麼樣的瀏覽器才夠現代化? IE 10+，最新版的 Chrome、Opera、Firefox 與 Safari。</small>

## Unobtrusive JavaScript

```html
<button onclick="document.getElementById('xyz').style.color = 'red';"></button>
```

這裡混合了結構與**行為**，我們來看看要怎麼改善這個情況。

### 分離結構與行為

```html
<!doctype html>
<html lang="en">
<head>
<meta charset="UTF-8">
             <meta name="viewport" content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
                         <meta http-equiv="X-UA-Compatible" content="ie=edge">
             <title>Document</title>
<!-- 樣式：本地的樣式元素與匯入的樣式表 -->
</head>
<body>
  <!--結構：HTML 結構元素-->
  <!--行為：本地的指令稿元素與匯入的指令稿元素-->
</body>
</html>
```

結構、樣式與行為都在各自應有的位置上。

所謂的 Unobtrusive JavaScript (不顯眼的 JavaScript) 策略，有助於網頁編輯者在網頁上進行有效區隔。作為一個令 Unobtrusive JavaScript 概念得以普及的程式庫，jQuery 適當的調整核心，更易於實作 Unobtrusive JavaScript。Unobtrusive JavaScript 認為，**任何** JavaScript 運算式或陳述句，只要被放到 HTML 頁面的 `<body>` 標籤中就不對，就算是作為 HTML 元素的屬性 (像是 onclick)，或者在頁面本體最底部以外的其他地方放置指令稿區塊，也都是不對的。

```html
<button id="test-button">Click Me</button>
```

但現在按鈕沒有什麼作用。我們來修正這個問題。

### 分離指令稿

依照現今最佳實踐，應該將指令稿移到文件下方，在本體標籤關閉之前 (`</body>`)：

```html
<script>
document.getElementById('test-button').addEventListener('click', function() {
  document.getElementById('xyz').style.color = 'red';
}, false);
</script>
```

因為你將指令稿放在頁面底部，就不需要像過去開發者做過的 (錯) 事那樣，將處理器繫結至 window 物件的 onload 事件了。或等待 DOMContentLoaded 事件在 HTML 文件完全載入與剖析完畢後就會觸發。load 事件則是 HTML 頁面與其相依資源完成載入之後觸發。將指令稿擺在頁面底部，瀏覽器剖析陳述句時，button 元素就會在了，因為它的標記已經剖析完了，所以能放心操作它了。

<small style='border: 1px solid #ccc; padding: 10px;'>注意>>> 基於效能考量，script 元素都應該放在文件本體底部。第一個理由是可以漸進呈現，第二理由是可以有更好的平行下載。第一理由背後動機是，**script 元素會阻斷 (block) 其後的所有內容呈現**。第二個理由背後原因是，**如果正在下載一個 script 元素，那麼瀏覽器就不會啟動其他下載，即使是不同的主機名稱**。</small>

前面的片段，addEventListener()，這在 IE 6 ~ 8 中不支援。jQuery 就能幫助你解決這個問題。

Unobtrusive JavaScript 是個強而有力的技巧，能清楚地區隔 web 應用程式中的職責，不過並非沒有任何代價。如果把指令稿直接放在按鈕標記中，需要的程式會比較少。Unobtrusive JavaScript 則可能增加指令稿的行數，還得依循一些規範，運用一些對客戶端指令稿來說良好的程式編寫模式。

這也不是什麼壞事；撰寫客戶端程式碼時，如果能付出與伺服器程式碼同等級的細心與重視，其實是件好事! 只不過沒有 jQuery 的話，就得多花費點功夫。

jQuery 團隊特別重視能否更輕鬆愉快地運用 Unobtrusive JavaScript 技巧，不用花費龐大心力或是撰寫大量程式碼就能完成相同目的。格言一直都是 "寫得更少，做得更多"。

## 安裝 jQuery

### 選擇正確的版本

2013年4月，jQuery 提出了 2.0 版本，該團隊決定放棄古老的 IE 6、7 與 8，將重點擺在未來的 web，而不是過去。

這個決定的結果是，刪除了瀏覽器相容性的程式碼，任務完成的結果是 -12% 容量、速度更快的基礎程式碼。雖然 1.x 與 2.x 是兩個不同的分支，它們的關係密切。jQuery 版本 1.10 與 2.0 之間有對應的特性，版本 1.11 與 2.1 也是，依此類推。

2014年10月，Dave Methvin，jQuery Foundation (jQuery 基金會) 總裁發表了一篇 blog，當中公開宣布將釋出 jQuery 新的重大版本：jQuery 3。正如 1.x 版本支援舊瀏覽器而 2.x 支援現代瀏覽器，jQuery 3 被分為兩個版本：jQuery Compat 3 是 1.x 的後繼者，而 jQuery 3 是 2.x 的後繼者。他進一步解釋：

> 從這些版本的發布開始，我們會重新調整我們的瀏覽器支援政策。jQuery 主套件保持小而輕巧，支援發佈當時常見的常青瀏覽器 (指那些經常自行升級至最新穩定版本的瀏覽器)。根據市場份額，我們也許會在這個套件中支援額外的瀏覽器。jQuery Compat 提供了更廣泛的瀏覽器支援，不過，代價是檔案會比較大，而且效能可能會比較低落。

新版本中，團隊也趁機拋棄了對某些瀏覽器的支援、修正臭蟲並改進許多特性。

| 瀏覽器  | jQuery 1         | jQuery 2         | jQuery Compat 3 | jQuery 3   |
|:--------|:-----------------|:-----------------|:----------------|:-----------|
| IE      | 6+               | 9+               | 8+              | 9+         |
| Chrome  | 目前與之前       | 目前與之前       | 目前與之前      | 目前與之前 |
| Firefox | 目前與之前       | 目前與之前       | 目前與之前      | 目前與之前 |
| Safari  | 5.1+             | 5.1+             | 7.0+            | 7.0+       |
| Opera   | 12.1x 目前與之前 | 12.1x 目前與之前 | 目前與之前      | 目前與之前 |
| iOS     | 6.1+             | 6.1+             | 7.0+            | 7.0+       |
| Android | 2.3、4.0+        | 2.3、4.0+        | 2.3、4.0+       | 2.3、4.0+  |

所謂目前與之前，意思是指 jQuery 新版發佈時，瀏覽器目前與之前的版本會根據 jQuery 新版本的發佈日期而不同。

另一個令人困惑的是壓縮版本，也稱為最小化的版本，目的是用於產品階段與開發階段。最小化程式庫的優點是容量上的縮減(空白、縮排、註解，還有縮減變數名稱)，可節省使用者頻寬。

本書使用 1.x 作為基礎。

這裡也談一下在自己的網站放置 jQuery 與使用 CDN 的不同。

### 使用 CDN 改進效能

在今日，透過**內容傳遞網路**來供應圖片與程式庫之類的檔案，以便改進網站效能，已經是個常見的實踐。CDN 是分散式系統的伺服器，以高可用性與效能來提供內容。瀏覽器同時間可以從主機下載一組固定的內容集合，通常是**四到八個**檔案。使用 CDN 供應的檔案是由不同主機提供，因此可以加速整個載入過程，增加同時間下載的檔案數量。此外，**今日許多網站都使用 CDN**，**因此需要的程式庫更有可能早存在於使用者的瀏覽器快取中**。因為還有許多因素介入的原因，使用 CDN 來載入 jQuery，並不保證在各種場合中都有更好的效能。我們建議是實際進行測試，確定哪個設定最符合你的情況。

在今日，有幾個可用來含括 jQuery 的 CDN，不過最可靠的是 jQuery CDN、Google CDN、與 Microsoft CDN。

```html
<script src="//code.jquery.com/jquery-1.11.3.min.js"></script>
```

這裡沒指定要用的協定，代表指定的是網站上正使用的相同協定。但要記得，若運行的網頁不是在 web 伺服器上，使用這個技巧就會產生錯誤。

然而，使用 CDN 不全然那麼美好。網際網路不會有 100% 正常運行的伺服器或網站，CDN也不例外。可改用以下這個聰明的方案：

```html
<script src="//code.jquery.com/jquery-1.11.3.min.js"></script>
<script>
window.jQuery || document.write('<script src="javascript/jquery-1.11.3.min.js"><\/script>');
</script>
```

藉由測試是否已定義 window 物件的 jQuery 特性來達到。如果測試失敗，你注入一段程式碼，這會載入本地主機上的複本。如果 jQuery 特性存在，可放心使用 jQuery 方法，又不用載入自己網站上的副本。

jQuery 特性是程式庫加入的，除了 jQuery 特性外，你也可找到一個 $ 快捷符號，這符號在本書與其他地方隨處可見。我們會使用 $，因為它與 jQuery 參考的是相同的物件，可從以下原始碼中獲得證實：

```javascript
window.jQuery = window.$ = jQuery;
```

現在你已曉得在 web 頁面中含括 jQuery 的方式了，現在要對架構來了解。

## jQuery 的架構

jQuery 檔案庫位於 GitHub 上，這是過去幾年來前端開發演進方式的完美範例。雖然與程式庫本身使用沒有太大關係，瞭解專業開發者組織其工作流程的方式以及運用的工具，一直都很重要。

開發團隊在 jQuery 的開發上，採用了今日前端舞台最新且最酷的技術，特別是這些：

- Node.js ─ **基於 Chrome 的 JavaScript 執行環境而建立的平台**，**可以把 JavaScript 作為伺服器端語言來執行**
- npm ─ Node.js 的官方**套件管理者程式**，用於安裝像是 Grunt 與其相關任務之類的套件
- Grunt ─ 可**自動化常見與重複性任務的任務執行器**，像是建構、測試與最小化
- Git ─ 自由、分散式的**版本控制系統**，用來追蹤程式碼的修改。讓開發者間更容易協作

另一方面，jQuery 原始碼遵循著非同步模組定義 (asynchronous module definition, AMD) 格式。對於可非同步載入的模組與其相依模組，AMD 格式是定義在這類模組的提案。這代表著，雖然看起來只是在使用一個 jQuery，它的原始碼實際上被分為數個檔案 (模組)，如下圖，這些檔案間的相依性是使用相依性管理器來管理─在這邊來說，就是 RequireJS。

![](https://imgur.com/kIclO3s.png)

這邊有些模組的例子：

- ajax ─ 包括所有 Ajax 函式，像是 ajax()、get() 與 post()
- deprecated ─ 包括目前已棄用但尚未移除的方法。版本不同，這模組中的東西會有所不同
- effects ─ 包括動畫相關方法，像是 animate() 與 slideUp()
- event ─ 包括可對瀏覽器事件設定事件處理器的方法，像是 on() 與 off()


將原始碼組織為模組還有另一個好處：可以建立自訂的 jQuery 版本，只包括你需要的模組。

### 建立自訂模組節省容量

jQuery 能讓你建立專屬的程式庫自訂版本，只包括需要的功能。這可以減輕程式庫的重量，獲得更好的效能改進。

能夠刪減不需要的模組很重要。這有助於改進網站效能，畢竟網站需要 jQuery 全部功能其實是值得懷疑的。

你可使用 Grunt 來建立自訂版本。假設你需要 1.11.3 的最小化版本，其中有著所有功能，不過不需要棄用的方法、特性與特效。要達到這個目的，你必須在本機上安裝 Node.js、Git 與 Grunt。完成安裝後，必須在命令列執行以下指令來複製 jQuery 的檔案庫：

```shell
git clone git://github.com/jquery/jquery.git
grunt custom:-deprecated,-effects
```

完成了! 在名稱為 dist 的目錄中，你會找到兩個自訂的 jQuery 版本，一個有最小化而一個沒有最小化。

然而這個方法並非沒有缺點。第一個問題來自於新版本 jQuery 的發佈。第二個問題會是在網站需要某個模組的特性，而先前卻沒有包括該模組時發生。在這些情況下，你必須再次執行先前說明的步驟 (通常只需要一個指令)，已建立新的自訂版本來包括新方法、臭蟲修正或者是缺少的模組。

現在，你知道置放程式庫以及建立自訂版本的方式了，該是探索 jQuery 基礎功能的時候了。

## jQuery 基礎功能

