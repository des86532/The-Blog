---
title: Ajax & Fetch 取得資料
comments: false
date: 2019-06-11
categories:
    - Javascript
tags:
    - ajax
    - fetch
---

![ajax and fetch](1_5u_6uR4Y26Y6y8bd4YKw8A.png)

## API

全名：「Application Programming Interface」，中文翻作應用程式介面。又稱為應用編程介面，就是軟體系統不同組成部分銜接的約定。

可以讓外部藉由串接我方的 API ，取得我方的資料。

舉例來說：

電腦上有一項標準叫做 USB 介面，所有廠商只要按照這一套標準來開發，就可以保證能夠連接電腦跟 USB 隨身碟

因為有一項標準叫做 USB 介面，當這套標準訂出來以後，所有廠商只要按照這一套標準來開發，就可以保證能夠連接電腦跟 USB 隨身碟。

API 也是這樣，只是變成程式跟程式之間的串接。例如說今天我寫程式需要讀取檔案好了，我要怎麼讀取檔案？讀取檔案是作業系統提供的功能，因此我可以去串接「讀取檔案的 API」，就可以在我的程式裡面也使用這個功能了。

例如說今天我想要讓我的網頁能夠用 Facebook 登入，那要怎麼辦？我就要去串接「Facebook 提供的 API」，就等於說是 Facebook 向外提供給大家的一套介面、一套標準，任何想要接入 Facebook 服務的開發者們，都可以遵循著那套規範拿到自己想要的資料，這個東西就叫做 API。

講到這邊，大家應該對 API 已經有一些 sense 了，我再多舉幾個例子：

1. 我想要抓到 flickr 上面的照片，所以我要去串接 flickr 的 API
2. Google 要開放讓其他 App 也能用 Google 登入驗證，所以 Google 要提供「Google 登入 API」
3. 我要抓 Twitch 上面現在有哪些頻道，所以要串 Twitch AP

> [輕鬆理解 Ajax 與跨來源請求](https://blog.techbridge.cc/2017/05/20/api-ajax-cors-and-jsonp/)

## 取得資料

### Ajax

全名：「Asynchronous Javascript And XML」，即非同步 JavaScript 及 XML。

Ajax 通過原生的 XMLHttpRequest 對象發出 Http 請求，得到服務器返回的數據後，在進行處理。

#### Ajax Works

![Ajax Works](0_jfPHIXjQqo4Ox0NO.gif)

1. 網頁觸發事件(點擊按鈕)
2. 創建 XMLHttpRequest 物件
3. XMLHttpRequest 物件發出請求給 server
4. server 處理請求
5. server 送回一個 response 給網頁
6. 讀取 response
7. 取得的資料，進行處理(渲染網頁)

#### Get Request
```
var data = null;
    var xhr = new XMLHttpRequest();
    xhr.addEventListener("readystatechange", function () {
        if (this.readyState === 4 && this.state == 200) {
            console.log(this.responseText);
        }
    });
    xhr.open("GET", "http://opendata2.epa.gov.tw/AQI.json");
    xhr.setRequestHeader("cache-control", "no-cache");
    xhr.send(data); //GET方法的時候，send傳送null
```

#### Post Request
```
var data = null;
    var xhr = new XMLHttpRequest();
    xhr.addEventListener("readystatechange", function () {
        if (this.readyState === 4 && this.state == 200) {
            console.log(this.responseText);
        }
    });
    xhr.open("POST", "http://opendata2.epa.gov.tw/AQI.json");
    xhr.setRequestHeader("cache-control", "no-cache");
    xhr.setRequestHeader('Content-Type', 'application/x-www-form-        urlencoded');
  xhr.send('username=admin&password=root');
```

open(method ,url ,boolean)，第三個參數為確認是否異步通知，默認為 true

如果第一個參數是 POST ，則需要將參數寫在 send() 方法裡面。send() 方法的參數可以是任何想送給服務器的數據。這時數據要以字符串的形式送給服務器，如：name=admin&password=root。

如果不設置請求頭，原生 AJAX 會默認使用 Content-Type 是 text/plain;charset=UTF-8 的方式發送數據。

## onreadystatechange property

readyStateHolds the status of the XMLHttpRequest.
- 0: request not initialized (還沒開始)
- 1: server connection established (讀取中)
- 2: request received (已讀取)
- 3: processing request (資訊交換中)
- 4: request finished and response is ready (資訊交換完成)status200: “OK”
- 403: “Forbidden”
- 404: “Page not found”

For a complete list go to the [Http Messages ReferencestatusTextReturns](https://www.w3schools.com/tags/ref_httpmessages.asp) the status-text (e.g. “OK” or “Not Found”)

Jquery 中的 Ajax 函數，就是對 xhr 的封裝
```
const corsUrl = 'https://cors-anywhere.herokuapp.com/';
    var settings = {
        "async": true,
        "crossDomain": true,
        "url": corsUrl + "http://opendata2.epa.gov.tw/AQI.json",
        "method": "GET",
        "headers": {
            "cache-control": "no-cache",
            "Postman-Token": "28b6edfb-cf02-4662-afc6-09d9fd2d7a68"
        }
    }
    console.log(settings.url)
    $.ajax(settings).done(function (response) {
        console.log(response);
    });
```
> [w3school — Ajax](https://www.w3schools.com/xml/ajax_intro.asp)
[深入理解ajax系列第一篇 — — XHR对象](https://www.cnblogs.com/xiaohuochai/p/6036475.html)
[Jquery](https://oscarotero.com/jquery/)

### Fetch

[Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) 提供了工具使操作 http pipeline 更加容易, 像是日常會用到的發送和接送資料都可以使用。並且有 global 的 [fetch()](https://developer.mozilla.org/en-US/docs/Web/API/WindowOrWorkerGlobalScope/fetch) 可以直接呼叫, 使開發能夠用更簡潔的語法取得非同步資料。

#### Get Request
```
fetch('http://opendata2.epa.gov.tw/AQI.json')
        .then(
            function (response) {
                if (response.status !== 200) {
                    console.log('Looks like there was a problem.                  
                              Status Code: ' +  response.status);
                    return;
                }
                // Examine the text in the response
                response.json().then(function (data) {
                    console.log(data);
                });
            }
        )
        .catch(function (err) {
            console.log('Fetch Error :-S', err);
        });
```

#### Post Request
```
fetch(url, {
    method: 'post',
    headers: {
      "Content-type": "application/x-www-form-urlencoded; charset=UTF-8"
    },
    body: 'foo=bar&lorem=ipsum'
  })
  .then(json)
  .then(function (data) {
    console.log('Request succeeded with JSON response', data);
  })
  .catch(function (error) {
    console.log('Request failed', error);
  });
```

## fetch 和 jQuery.ajax() 有兩個主要的差異:

fetch() 回傳的 promise 物件, resolve 和 reject 的使用方式有差異, 當遇到 HTTP Status 404, 500 時會使用 resolve 但會將 status 的值從 ok 變為 false， 也就是說，它只會在網路出現問題或是被阻止進行Request(要求)時，才會變成已拒絕(rejected)狀態，其他都是已實現(fulfilled)。

fetch方法預設是不會傳送任何的認証証書(credentials)例如cookie到伺服器上的，如果網站依賴 session 會導致請求回傳未經認證，要加上傳送cookie可以用fetch(url, {credentials: 'include'})的語法來設置。

> [developers-google](https://developers.google.com/web/updates/2015/03/introduction-to-fetch)
[MDN — Fetch](https://developer.mozilla.org/zh-TW/docs/Web/API/Fetch_API/Using_Fetch#使用_Fetch_發送請求_(_request_))
[Gitbooks — ES6](https://eyesofkids.gitbooks.io/javascript-start-from-es6/content/part4/promise.html)
[MDN — XMLHttpRequest](https://developer.mozilla.org/zh-TW/docs/Web/API/XMLHttpRequest)
[《你不知道的 XMLHttpRequest》](https://segmentfault.com/a/1190000008950789)
[fetch API 和 Ajax（XMLHttpRequest）的差异](https://www.jianshu.com/p/373c348737f6)