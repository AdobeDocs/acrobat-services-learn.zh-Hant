---
title: 數位文件發佈
description: 學習如何使用 Adobe PDF 嵌入 API 在網頁中顯示嵌入的 PDF 文件
feature: Use Cases
role: Developer
level: Intermediate
type: Tutorial
jira: KT-8090
thumbnail: KT-8090.jpg
exl-id: 3aa9aa40-a23c-409c-bc0b-31645fa01b40
TQID: https://experienceleague.adobe.com/ps-wxzaqHNuBwOlfWDDEmOamM3ZOmP-4Ys1H4X--Gk0
product_v2:
  - id: acdc2bde-2937-4877-90d9-031dd66278c9
feature_v2:
  - id: b1809bd0-a86b-4991-8083-2e3b517fc3b8
  - id: c4d07275-6387-4756-8bf7-681e581ffd27
subfeature_v2:
  - id: c4b1e8f2-d9a8-4792-b5e4-be52bd870028
  - id: c6f72a9c-54c4-4933-93c9-d7c656ff1f14
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
topic_v2:
  - id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dc
  - id: bce87dde-a4ab-44c9-8a18-ad66e4ddb377
source-git-commit: 0110d2606056220c4236fe2f0e3afbfc112746e7
workflow-type: tm+mt
source-wordcount: 1968
ht-degree: 0%

---

# 數位文件發佈

![使用案例英雄卡池](assets/UseCaseDigitalHero.jpg)

電子文件無處不在——事實上，全球可能 [有數兆份 PDF](https://itextpdf.com/en/blog/technical-notes/do-you-know-how-many-pdf-documents-exist-world) ，且這個數字每天都在增加。 透過在網頁中嵌入 PDF 檢視器，使用者能在不重新設計 HTML 和 CSS 或阻礙網站存取的情況下瀏覽文件。

讓我們來探討一個受歡迎的情境。 一家公司會在 [網站上發布白皮書](https://developer.adobe.com/document-services/use-cases/content-publishing/digital-content-publishing)為他們的應用程式和服務提供背景說明。 網站行銷人員希望更深入了解用戶如何與其 PDF 內容互動，並將其融入網站與品牌中。 他們決定將白皮書作為 [封閉內容](https://whatis.techtarget.com/definition/gated-content-ungated-content#:~:text=Gated%20content%20is%20online%20materials,about%20their%20jobs%20and%20organizations。)發佈，並控制誰能下載。

## 你可以學到什麼

在這個實作教學中，學習如何使用 [免費且易於使用的 Adobe PDF 嵌入 API](https://developer.adobe.com/document-services/apis/pdf-embed) 在網頁中顯示嵌入的 PDF 文件。 這些範例會使用一些JavaScript、Node.js、Express.js、HTML 和 CSS。 你可以在 [GitHub](https://www.google.com/url?q=https://github.com/marcelooliveira/EmbedPDF/tree/main/pdf-app&sa=D&source=editors&ust=1617129543031000&usg=AOvVaw2rzSwYuJ_JI7biVIgbNMw1) 上查看完整的專案程式碼。

## 相關 API 與資源

* [PDF 嵌入 API](https://www.adobe.com/devnet-docs/dcsdk_io/viewSDK/index.html)

* [PDF 服務 API](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html)

* [專案程式碼](https://www.google.com/url?q=https://github.com/marcelooliveira/EmbedPDF/tree/main/pdf-app&sa=D&source=editors&ust=1617129543031000&usg=AOvVaw2rzSwYuJ_JI7biVIgbNMw1)

## 建立節點網頁應用程式

我們先用 Node.js 和 Express 建立一個網站，使用漂亮的範本並提供多個 PDF 下載。

首先，下載 [並安裝Node.js](https://nodejs.org/en/download/)。

若要以簡約的網頁應用程式結構輕鬆建立Node.js專案，請安裝應用程式產生器工具 `` `express-generator` ``。

```
npm install express-generator -g
```

接著，建立名為 pdf-app 的新 Express 應用程式，選擇作為檢視引擎。

```
express pdf-app --view=ejs
```

現在，移到 \\pdf-app 目錄並安裝所有專案相依關係。

```
cd pdf-app
npm install
```

接著啟動本地網頁伺服器並執行應用程式。

```
npm start
```

最後，請在 <http://localhost:3000>.

![基本網站截圖](assets/ddp_1.png)

你現在擁有一個基本的網站。

## 渲染白皮書資料

要將白皮書發布至網站，白皮書資料會被定義並準備在網站上以顯示這些文件。 首先，在專案根建立一個新的 \\data 資料夾。 可用白皮書的資訊來自一個名為 [data.json](https://github.com/marcelooliveira/EmbedPDF/blob/main/pdf-app/data/data.json)的新檔案，該檔案被放入資料資料夾中。

為了讓網頁應用程式看起來漂亮且精緻，請安裝 [Bootstrap](https://getbootstrap.com/) 和 [Font Awesome](https://fontawesome.com/) 的前端函式庫。

```
npm install bootstrap
npm install font-awesome
```

打開 app.js 檔案，將這些目錄作為靜態檔案的來源，將它們放在現有 `` `express.static` `` 檔案行之後。

```
app.use(express.static(path.join(__dirname, '/node_modules/bootstrap/dist')));
app.use(express.static(path.join(__dirname, '/node_modules/font-awesome')));
```

要包含 PDF 文件，請在專案的 \\public 資料夾下方建立一個名為 \\pdfs 的資料夾。 你不用自己建立 PDF 和縮圖，可以從這個 [GitHub 倉庫資料夾](https://github.com/marcelooliveira/EmbedPDF/tree/main/pdf-app/public) 複製到 \\pdfs 和 \\image 資料夾。

\\public\\pdfs 資料夾現在包含以下 PDF 文件：

![PDF 檔案圖示截圖](assets/ddp_2.png)

而 \\public\\images 資料夾應該包含每份 PDF 文件的縮圖：

![PDF 縮圖截圖](assets/ddp_3.png)

現在，打開 \\routes\\index.js 檔案，裡面有首頁路由的邏輯。 要使用 data.json 檔案中的白皮書資料，必須載入負責存取及互動檔案系統的 Node.js 模組。 接著，在 \\routes\index.js 檔案的第一行宣告 `fs` 常數，如下：

```
const fs = require('fs');
```

接著，讀取並解析data.json檔案，並將其存入 papers 變數中：

```
let rawdata = fs.readFileSync('data/data.json');
let papers = JSON.parse(rawdata);
```

現在修改這行，呼叫索引視圖的渲染方法，並將論文集合作為索引視圖的模型傳入。

```
res.render('index', { title: 'Embedding PDF', papers: papers });
```

要在首頁呈現白皮書集合，打開 \\views\\index.ejs 檔案，將現有程式碼替換成你專案索引檔[&#128279;](https://github.com/marcelooliveira/EmbedPDF/blob/main/pdf-app/views/index.ejs)的程式碼。

現在，重新執行 NPM 開始並開啟 <http://localhost:3000> ，查看你所有可用的白皮書收藏。

![白皮書縮圖截圖](assets/ddp_4.png)

接下來的章節將介紹網站的美化，以及使用 [PDF 嵌入 API](https://developer.adobe.com/document-services/apis/pdf-embed) 來顯示 PDF 文件到網頁。 PDF 嵌入 API 是免費使用的——你只需要取得 API 憑證。

## 取得 PDF 嵌入 API 憑證

要取得免費的 PDF 嵌入 API 憑證，請在註冊新帳號或登入現有帳號後，造訪 [「開始](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html) 」頁面。

點擊 **建立新憑證** ，然後 **開始：**

![如何建立新憑證的截圖](assets/ddp_5.png)

此時，如果你還沒有免費帳號，系統會要求你註冊一個免費帳號。

選擇 **PDF 嵌入 API**，然後輸入你的憑證名稱和應用程式網域。 用 **localhost** 網域，因為要在本地測試網頁應用程式。

![為 PDF 嵌入 API 建立新憑證的截圖](assets/ddp_6.png)

點擊 **建立憑證** 按鈕即可存取您的 PDF 憑證並取得客戶端 ID（API 金鑰）。

![如何複製新帳號的截圖](assets/ddp_7.png)

在你的Node.js專案中，建立一個名為 . 的檔案。ENV 在應用程式的根目錄中，並宣告 PDF Embed Client ID 的環境變數，並以前一步的 API KEY 憑證值來指定。

```
PDF_EMBED_CLIENT_ID=**********************************************
```

之後你會用這個客戶端 ID 來存取 PDF 嵌入 API。 安裝 dotenv 套件，利用 Node.js 程式碼存取這個環境變數。

```
npm install dotenv
```

現在，打開app.js檔案，並在檔案頂端加上以下一行，這樣Node.js就能載入 dotenv 模組：

```
require('dotenv').config();
```

## 在網頁應用程式中顯示 PDF

現在使用 PDF 嵌入 API 在網站上顯示 PDF。 打開即時 [的 PDF 嵌入 API 示範](https://documentcloud.adobe.com/view-sdk-demo/index.html#/view/FULL_WINDOW/Bodea%20Brochure.pdf)。

![即時 PDF 嵌入 API 示範截圖](assets/ddp_8.png)

在左側面板，您可以選擇最適合您網站需求的嵌入模式：

* **完整視窗**：PDF 涵蓋整個網頁空間

* **尺寸容器**：PDF 會顯示在網頁內，一次一頁，以有限大小的 div 呈現

* **內行**：整個 PDF 會顯示在網頁內的 div 中

* **Lightbox**：PDF 會以圖層形式顯示在網頁上方

建議使用內嵌模式來撰寫白皮書，之後再使用程式碼產生器將 PDF 嵌入應用程式。

## 建立內嵌模式頁面

要將 PDF 檢視器嵌入網頁並同時顯示所有頁面，請使用內嵌模式建立新頁面。

使用 EJS 檢視引擎在檔案 \\views\\in-line.ejs 中建立一個新檢視。

```
<! html DOCTYPE >
<html>
<head>
<title>
<%= title %>
</title>
<link rel='stylesheet' href='/stylesheets/style.css' />
<link rel='stylesheet' href='/css/bootstrap.min.css'/>
<link rel='stylesheet' href='/css/font-awesome.min.css' />
<style type="text/css">
p {
font-family: 'Gill Sans', 'Gill Sans MT', Calibri, 'Trebuchet MS', sans-serif
}
</style>
</head>
<body class="m-0">
<div>
<main>
<div class="row">
<div class="col-sm-3"></div>
<div class="col-sm-6">
<h3>
<p class="text-center">Grow your business, establish your brand,<br
/>
```

並且把顧客放在第一位。

```
</p>
</h3>
<div>
<p class="text-center">Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do<br />
eiusmod tempor incididunt ut labore et dolore</p>
</div>
</div>
</main>
<footer>
<div class="row">
<div class="col-sm-3"></div>
<div class="col-sm-6">
<p class="text-center">Bodea Inc. Your trusted partner since 2008</p>
</div>
</div>
</footer>
</div>
</div>
</body>
</html>
```

接著，修改 \\views\\index.ejs，建立一個按鈕來開啟內嵌檢視。

```
<div class="card-body">
<h5 class="card-title">
<span>
<%= paper.title %>
</span>
</h5>
<p>
<a class="btn btn-sm btn btn-danger" href="/in-line/<%=
paper.id %>">
<span type="button"></span>
<span class="fa fa-file-pdf-o"></span>&nbsp;View Document</button>
</a>
</p>
</div>
```

打開 app.js 檔案，並在 indexRouter 宣告後宣告新路由器：

```
var indexRouter = require('./routes/index');
var inLineRouter = require('./routes/in-line');
```

接著在 app.use（&#39;/&#39;， indexRouter） 之後加上這個程式碼;要將內嵌模式視圖與其路由器關聯：

```
app.use('/', indexRouter);
app.use('/in-line', inLineRouter);
```

現在，在 \routes 下建立一個新的 in-line.js 檔案，建立新的路由器邏輯。 包含 Express，一個能啟用網頁應用後端的節點模組。

```
var express = require('express');
const fs = require('fs');
var router = express.Router();
```

接著，建立一個端點來處理特定白皮書 ID 的 GET 請求，並呈現 in-line.ejs 的檢視。

```
router.all('/:id', function(req, res, next) {
let rawdata = fs.readFileSync('data/data.json');
let papers = JSON.parse(rawdata);
let paper = papers.filter(p => p.id == parseInt(req.params.id))[0];
res.render('in-line', { title: paper.title, paper: paper });
});
module.exports = router;
```

再看 [一次現場示範](https://documentcloud.adobe.com/view-sdk-demo/index.html#/view/FULL_WINDOW/Bodea%20Brochure.pdf) ，可以自動產生 PDF 嵌入 API 程式碼。 從左側面板點擊 **「In-Line** 」：

![即時 PDF 嵌入 API 示範截圖](assets/ddp_8.png)

點擊 **「產生程式碼** 」以查看顯示尺寸容器 PDF 檢視器所需的 HTML 程式碼。

![程式碼預覽截圖](assets/ddp_9.png)

點選 **複製程式碼** ，然後把程式碼貼到 in-line.ejs 檔案中。

```
<div>
<p class="text-center">Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do<br />
eiusmod tempor incididunt ut labore et dolore</p>
</div>
<div class="row align-items-center border border-primary">
<div id="adobe-dc-view" style="width: 800px;"></div>
<script src="https://documentcloud.adobe.com/view-sdk/main.js"></script>
<script type="text/javascript">
document.addEventListener("adobe_dc_view_sdk.ready", function(){
var adobeDCView = new AdobeDC.View({clientId: "<YOUR_CLIENT_ID>", divId: "adobe-dc-view"});
adobeDCView.previewFile({
content:{location: {url: "https://documentcloud.adobe.com/view-sdk-demo/PDFs/Bodea Brochure.pdf"}},
metaData:{fileName: "Bodea Brochure.pdf"}
}, {embedMode: "IN_LINE"});
});
</script>
</div>
```

然而，文件參數仍是硬編碼的。 我們用 EJS 括號語法（\&lt;%= someValue %\>）來依照白皮書模型資料渲染頁面。

```
<div id="adobe-dc-view" style="width: 800px;"></div>
<script src="https://documentcloud.adobe.com/view-sdk/main.js"></script>
<script type="text/javascript">
document.addEventListener("adobe_dc_view_sdk.ready", function () {
var adobeDCView = new AdobeDC.View({ clientId: "<%=process.env.PDF_EMBED_CLIENT_ID %>", divId: "adobe-dc-view" });
adobeDCView.previewFile({
content: { location: { url: "<%= paper.pdf %>" } },
metaData: { fileName: "<%= paper.fileName %>" }
}, {
embedMode: "IN_LINE"
});
});
</script>
```

現在用 npm start 指令執行應用程式，並從 <http://localhost:3000>.

![PDF 白皮書縮圖截圖](assets/ddp_10.png)

最後，選擇一份白皮書並點擊 **「檢視文件** 」開啟新頁面，內嵌 PDF：

![PDF 白皮書截圖 &#x200B;](assets/ddp_11.png)

請注意現在已經有下載PDF和列印PDF選項了。

![下載與列印選項截圖](assets/ddp_12.png)

你要在後台控制這些旗標。 之後你可以根據使用者身份實作授權控制，並根據你的商業規則限制存取。 這裡不需要那麼複雜，所以我們只要修改 \\routes\\in-line.js，讓模型物件中包含已驗證和權限屬性即可。

```
let authenticated = false;
res.render('in-line', {
title: paper.title,
paper: paper,
authenticated: authenticated,
permissions: {
showDownloadPDF: true,
showPrintPDF: true,
showFullScreen: true
}
});
```

接著，修改 \\views\\in-line.ejs，讓你的網頁能渲染來自後端的旗標值。

```
embedMode: "IN_LINE",
showDownloadPDF: <%= permissions.showDownloadPDF %>,
showPrintPDF: <%= permissions.showPrintPDF %>,
showFullScreen: <%= permissions.showFullScreen %>
Now, open the in-line.js route file and modify it to disallow the printing, downloading, and full-screen controls.
permissions: {
showDownloadPDF: false,
showPrintPDF: false,
showFullScreen: false
}
```

接著，重新執行應用程式，看看這個變更在 PDF 檢視器中如何反映。

![PDF 檔案截圖](assets/ddp_13.png)

## 創建有門檻內容

根據最終用戶情境，公司網站的行銷人員希望更了解用戶如何與其基於 PDF 的內容互動，並將內容與其網頁及品牌的其他部分整合。

我們的重點是 PDF 嵌入，所以你不需要建立使用者驗證功能。 相反地，只要用一個簡單的假付費牆，使用一個接受部分用戶資訊的網頁表單，然後在用戶提交表單後顯示 PDF 文件即可。

請將 \\routes\\in-line.js 檔案替換為以下內容，以提供包含使用者資訊的檢視模型：

```
var express = require('express');
const fs = require('fs');
var router = express.Router();
router.all('/:id', function(req, res, next) {
let rawdata = fs.readFileSync('data/data.json');
let papers = JSON.parse(rawdata);
let paper = papers.filter(p => p.id == parseInt(req.params.id))[0];
let authenticated = false;
let user = {};
if (req.body.firstName) {
user = {
firstName: req.body.firstName,
lastName: req.body.lastName,
jobTitle: req.body.jobTitle,
email: req.body.email,
};
authenticated = true;
}
res.render('in-line', {
title: paper.title,
paper: paper,
user: user,
authenticated: authenticated,
permissions: {
showDownloadPDF: false,
showPrintPDF: false,
showFullScreen: false
}
});
});
module.exports = router;
```

接著用下面的程式碼替換 \\views\\in-line.ejs 的內容。 它會顯示使用者資料表單或 PDF 檢視器，視是否為認證使用者而定。

```
<!DOCTYPE html>
<html>
<head>
<title>
<%= title %>
</title>
<link rel='stylesheet' href='/css/bootstrap.min.css'/>
<link rel='stylesheet' href='/css/font-awesome.min.css' />
<style type="text/css">
p {
font-family: 'Gill Sans', 'Gill Sans MT', Calibri, 'Trebuchet MS', sans-serif
}
</style>
</head>
<body class="m-0">
<% if (authenticated) { %>
<header class="bg-dark text-white">
<div class="text-right mr-4">Hello, <%= user.firstName %> <%= user.lastName%></div>
</header>
<% } %>
<div>
<main>
<div class="row">
<div class="col-sm-3"></div>
<div class="col-sm-6">
<h3>
<p class="text-center">Grow your business, establish your brand,<br
/>
```

並且把顧客放在第一位。

```
</p>
</h3>
<div>
<p class="text-center">Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do<br />
eiusmod tempor incididunt ut labore et dolore</p>
</div>
<% if (!authenticated) { %>
<div class="row">
<form method="POST" class="center-panel text offset-md-3 col-md-6 border">
<fieldset class="offset-md-1">
<legend>Submit your info to<br/>access the whitepaper</legend>
<p><input name="firstName" placeholder="first name"/></p>
<p><input name="lastName" placeholder="last name"/></p>
<p><input name="jobTitle" placeholder="job title"/></p>
<p><input name="email" placeholder="email"/></p>
<p><button type="submit" class="btn btn-sm btn btn-primary">Submit</button></p>
</fieldset>
</form>
</div>
<% } %>
<% if (authenticated) { %>
<div class="row align-items-center border border-primary">
<div id="adobe-dc-view" style="width: 800px;"></div>
<script src="https://documentcloud.adobe.com/view-sdk/main.js"></script>
<script type="text/javascript">
document.addEventListener("adobe_dc_view_sdk.ready", function () {
var adobeDCView = new AdobeDC.View({ clientId: "<%=process.env.PDF_EMBED_CLIENT_ID %>", divId: "adobe-dc-view" });
adobeDCView.previewFile({
content: { location: { url: "<%= paper.pdf %>" } },
metaData: { fileName: "<%= paper.fileName %>" }
}, {
embedMode: "IN_LINE",
showDownloadPDF: <%= permissions.showDownloadPDF %>,
showPrintPDF: <%= permissions.showPrintPDF %>,
showFullScreen: <%= permissions.showFullScreen %>
});
});
</script>
<% } %>
</div>
</div>
</main>
<footer>
<div class="row">
<div class="col-sm-3"></div>
<div class="col-sm-6">
<p class="text-center">Bodea Inc. Your trusted partner since 2008</p>
</div>
</div>
</footer>
</div>
</div>
</body>
</html>
```

![門檻內容截圖](assets/ddp_14.png)

網站訪客現在只能在提交資料後存取 PDF：

![嵌入檢視器中 PDF 內容的截圖](assets/ddp_15.png)

## 促成事件

讓我們看看如何輕鬆將 PDF 檢視器事件整合到你的應用程式中，為行銷人員收集分析數據。 要使用 PDF EmbedAPI 擴充您的檢視器，請在宣告 adobeDCView 變數後，並在呼叫 previewFile 方法前，先加入以下程式碼：

```
var adobeDCView = new AdobeDC.View({ clientId: "<%=process.env.PDF_EMBED_CLIENT_ID %>", divId: "adobe-dc-view" });
adobeDCView.registerCallback(
AdobeDC.View.Enum.CallbackType.EVENT_LISTENER,
function(event) {
console.log(event);
},
{ enablePDFAnalytics: true }
);
```

接著，重新啟動應用程式，並開啟瀏覽器的開發者工具查看事件資料。

![程式碼截圖](assets/ddp_16.png)

你可以將這些資料傳送給 [Adobe Analytics](https://developer.adobe.com/document-services/docs/overview/pdf-embed-api) 或其他分析工具。

## 後續步驟

[!DNL Acrobat Services] API 幫助開發者輕鬆解決以 PDF 為中心的工作流程解決數位出版的挑戰。 你已經看過如何建立一個範例 Node 網頁應用程式來顯示一組白皮書。 接著，取得 [免費的 API 憑證](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html) ，並建立對白皮書的限制存取權限，白皮書可以四種 [嵌入模式](https://documentcloud.adobe.com/view-sdk-demo/index.html#/view/FULL_WINDOW/Bodea%20Brochure.pdf)之一顯示。

將此工作流程整合起來，有助於 [假設的行銷人員](https://developer.adobe.com/document-services/use-cases/content-publishing/digital-content-publishing) 收集潛在客戶聯絡資訊，以換取白皮書下載及查看與 PDF 互動的統計資料。 你可以將這些功能整合到網站中，以推動並監控用戶互動。

如果你是 Angular 或 React 開發者，可能會喜歡嘗試 [更多](https://github.com/adobe/pdf-embed-api-samples) 範例，介紹如何將 PDF 嵌入 API 整合到 React 和 Angular 專案中。

Adobe 讓你能透過創新解決方案打造端到端的客戶體驗。 免費試用 [Adobe PDF 嵌入 API](https://developer.adobe.com/document-services/apis/pdf-embed/) 。 想探索還能做什麼，可以試試 Adobe PDF Services API 搭配 [付費（Pay-as-you-gopr](https://developer.adobe.com/document-services/pricing/main) [）糖霜](https://developer.adobe.com/document-services/pricing/main)。

[今天就開始](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html) 使用 [!DNL Adobe Acrobat Services] API。
