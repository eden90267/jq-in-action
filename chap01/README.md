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