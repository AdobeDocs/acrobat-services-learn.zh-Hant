---
title: 職務過帳
description: 瞭解如何為求職者和雇主開發平穩、一致的Web體驗
feature: Use Cases
role: Developer
level: Intermediate
type: Tutorial
jira: KT-8092
thumbnail: KT-8092.jpg
exl-id: 0e24c8fd-7fda-452c-96f9-1e7ab1e06922
source-git-commit: bd53d86abb0e5f9ee302c39e07c00101e5a1f8ed
workflow-type: tm+mt
source-wordcount: '1443'
ht-degree: 0%

---

# 職務過帳

![使用案例英雄橫幅](assets/UseCaseJobHero.jpg)

在操作具有多個用戶的網站時，設計一種能夠確保每個人都能獲得流暢體驗的體驗至關重要。

想像一下以下情形：您有一個允許雇主[上傳職務發佈](https://developer.adobe.com/document-services/use-cases/content-publishing/job-posting)的網站。 對於求職者來說，以一致的格式輕鬆查看與帖子相關的所有文檔是非常方便的。 但是，雇主可以方便地以他們碰巧擁有的任何檔案格式附加資訊。 為方便這兩類用戶，您可以自動將所有上載的文檔轉換為PDF，並將它們嵌入到電子公告中。

## 你能學到的

本操作教程將介紹一個Node.js示例，該示例使用[!DNL Adobe Acrobat Services]及其[Node.js SDK](https://www.npmjs.com/package/@adobe/documentservices-pdftools-node-sdk)將這些功能添加到作業發佈站點。 這創造了一個更易於使用、對雇主和求職者都更有吸引力的網站。 這是[完成](https://github.com/contentlab-io/adobe_job_posting) [項目代碼](https://github.com/contentlab-io/adobe_job_posting)，以備您在閱讀時繼續操作。

要啟動，請設定一個簡單的基於Express的Node.js Web應用程式。 [Express](https://expressjs.com/)是極簡的Web應用程式框架，提供路由和模板等功能。 應用程式的代碼在[GitHub](https://github.com/contentlab-io/adobe_job_posting)上可用。 另外，請安裝[PostgreSQL資料庫](https://www.postgresql.org/)，並將其設定為儲存PDF。

## 相關[!DNL Acrobat Services]個API

* [PDF嵌入API](https://www.adobe.com/devnet-docs/dcsdk_io/viewSDK/index.html)

* [PDF服務API](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html)

## 建立AdobeAPI憑據

首先，您必須[為Adobe PDF嵌入API（免費使用）和Adobe PDF服務API（免費6個月）建立憑據](https://www.adobe.com/go/dcsdks_credentials)，然後[按使用付費](https://developer.adobe.com/document-services/pricing/main)為每個文檔交易僅\$0.05)。 為PDF服務API建立憑據時，選擇「建立個性化代碼示例」選項。 保存ZIP檔案，並將pdftools-api-credentials.json和private.key解壓到Node.js Express項目的根目錄。

您還需要API密鑰，以便免費使用Embed API。 從[項目](https://developer.adobe.com/console/projects)，轉到您建立的項目。 然後，按一下&#x200B;**添加到項目**，然後選擇&#x200B;**API**。 最後，按一下&#x200B;**PDF嵌入API**。

指定PDF嵌入API的域。 API密鑰必須是公共的（在瀏覽器執行的代碼中查找）。 通過指定域，可確保其他域中的其他人無法使用API密鑰。

不能將&quot;localhost&quot;用作域。 指定域（如「testing.local」），並編輯電腦上的hosts檔案以將該域重定向到127.0.0.1（即您的電腦）。 然後，您可以在testing.local:3000上測試應用程式，而不是在localhost:3000上測試應用程式。 完成後，在項目頁上查找PDFEmbed API的API鍵。

## 添加上載表單和處理程式

使用工作的Express應用程式和API憑據，您還需要一個表單，使用戶能夠將其文檔上載到網站。 為此目的編輯index.jade模板。

為上載的作業過帳的名稱和包含詳細資訊的文檔建立輸入欄位。

在模板的內容塊內，添加以下表單：

```
extends layout

block content
  h1= title

  form(action="/upload", enctype="multipart/form-data", method="POST")
    label Job posting name:&nbsp;
    input(type="text", name="name", required="required")
    br
    br
    label Describing document:&nbsp;
    input(type="file", name="attachment", required="required")
    br
    br
    input(type="submit", value="Submit job posting")
```

接下來，將POST請求的處理程式添加到/upload操作。 然後，將/upload的路由添加到routes/index.js檔案。 您可以為此路由建立新檔案，但必須更新app.js檔案以反映新檔案。 在此路由處理程式內，可以訪問給定名稱和上載的檔案。

```
router.post('/upload', async function (req, res, next) {
    const name = req.body.name;
    const fileContents = req.files.attachment.data;

    // code to work with the uploaded document
  });
```

該函式是非同步的，因此您可以在函式中使用await關鍵字，這在調用執行API調用的方法時非常方便。

![作業發佈網站的螢幕截圖](assets/jobs_1.png)

## 使用PDF服務API

在使用PDF服務API之前，必須將以下導入添加到路由檔案的頂部：

```
const PDFToolsSdk = require('@adobe/documentservices-pdftools-node-sdk');
  const { Readable } = require('stream');
```

在導入下，您可以載入API憑據並建立[執行內容](https://www.javascripttutorial.net/javascript-execution-context/)。 由於您可以為不同的操作重新使用執行上下文，因此只執行一次是合理的。

```
  const credentials = PDFToolsSdk.Credentials
  .serviceAccountCredentialsBuilder()
  .fromFile("pdftools-api-credentials.json")
  .build();

  const executionContext = PDFToolsSdk.ExecutionContext.create(credentials);
```

現在，返回到在`router.post`塊中的注釋處在請求處理程式中寫入代碼。 首先將文檔轉換為PDF。

```
  const createPdfOperation = PDFToolsSdk.CreatePDF.Operation.createNew();

  const input = PDFToolsSdk.FileRef.createFromStream(Readable.from(fileContents),
  req.files.attachment.mimetype);

  createPdfOperation.setInput(input);

  let result = await createPdfOperation.execute(executionContext);

  result.saveAsFile('output-pdf' + new Date().getTime() + '.pdf');
  return res.send('success!');
```

大多數操作都採取相同的四個步驟。 首先，使用相應類的createNew方法初始化操作類型。 然後，為操作建立輸入，即FileRef。 後續操作可跳過此步驟，因為操作的結果也是FileRef。 對於此初始操作，從上載檔案的位元組建立FileRef。 第三，必須將輸入分配給操作。 最後，執行該操作，該執行上下文作為執行方法中的參數。 此方法返回Promise，以便您可以等待結果。

代碼將返回的PDF保存到檔案，並向瀏覽器發送簡單的「成功」響應。 檔案名的「日期」部分保證唯一的檔案名。 如果目標檔案存在，則saveAsFile將返回錯誤。

## 將影像轉換為文本並壓縮PDF

現在，使用光學字元識別(OCR)將影像轉換為文本，然後壓縮結果。 使用與CreatePDF操作類似的OCR和CompressPDF操作執行此操作。 在`router.post`中將以下內容添加到路由檔案：

```
  const name = req.body.name;
  const fileContents = req.files.attachment.data;

  const createPdfOperation = PDFToolsSdk.CreatePDF.Operation.createNew();
  const input = PDFToolsSdk.FileRef.createFromStream(Readable.from(fileContents),
  req.files.attachment.mimetype);
  createPdfOperation.setInput(input);

  let result = await createPdfOperation.execute(executionContext);

  const ocrOperation = PDFToolsSdk.OCR.Operation.createNew();
  ocrOperation.setInput(result);
  result = await ocrOperation.execute(executionContext);

  const compressPdfOperation = PDFToolsSdk.CompressPDF.Operation.createNew();
  compressPdfOperation.setInput(result);
  result = await compressPdfOperation.execute(executionContext);

  result.saveAsFile('output-pdf' + new Date().getTime() + '.pdf');
  return res.send('success!');
```

只需執行一次此操作，因為結果是FileRef，代碼可以傳遞給setInput。

最好的選擇是將檔案保存在硬碟上並返回過度簡化的HTTP響應。 相反，將PDF儲存在資料庫中，並顯示一個網頁，該網頁使用Adobe的免費PDF嵌入API嵌入PDF。 這樣，雇主的招聘帖子或手冊就可以在網站上看到，供求職者查找和查看，並附上公司徽標和其他設計要素。

## 將PDF儲存在資料庫中

將PDF儲存在PostgreSQL資料庫中。 獲取要連接到Node.js中Postgres的node-postgres包。 安裝流緩衝區包，因為在某個時刻，必須將PDF的內容儲存在緩衝區中，而FileRef只適用於流。 因此，使用流緩衝區包將內容寫入緩衝區。

```
npm install pg stream-buffers
```

現在，為職務發佈建立資料庫表。 它需要一列作為唯一標識符，一列作為名稱，一列作為附加PDF。 可以通過Postgres命令行介面(CLI)建立資料庫表：

```
CREATE TABLE job_postings (id TEXT PRIMARY KEY, name TEXT NOT NULL, attachment
BYTEA NOT NULL);
```

返回到Node.js檔案。 在檔案頂部添加一些導入：

```
  const { Client } = require('pg');
  const streamBuffers = require('stream-buffers');
```

要將PDF儲存在資料庫表中，請修改上載函式。 將最後兩行（saveAsFile和send）替換為此代碼段：

```
  const pgClient = new Client();
  pgClient.connect();

  const id = Math.random().toString(36).substr(2, 6); // not securely random at all,
  but serves the purpose for this demo

  const writableStream = new streamBuffers.WritableStreamBuffer();
  writableStream.on("finish", async () => {    
    await pgClient.query("INSERT INTO job_postings VALUES ($1, $2, $3)", [
      id,
      name,
      writableStream.getContents()
    ]);
    res.redirect(`/job/${id}`);
  })
  result.writeToStream(writableStream);
```

要寫入內容，請建立WritableStreamBuffer。 使用完成事件，是時候執行SQL查詢了。 node-postgres包自動將Buffer參數轉換為BYTEA格式。 查詢將用戶重定向到稍後建立的終結點/job/{id}。

對於PDF嵌入API，您還需要一個僅返回PDF內容的終結點：

```
  router.get('/pdf/:id', async function (req, res, next) {
    const id = req.params.id;
 
    const pgClient = new Client();
    pgClient.connect();

  const pgResult = await pgClient.query("SELECT attachment FROM job_postings WHERE id
  = $1", [id]);
  const buffer = pgResult.rows[0].attachment;
  res.type('pdf');
    return res.send(buffer);
  });
```

## 嵌入PDF

現在，建立/job/{id}終結點，該終結點呈現包含請求的作業過帳名稱的模板和嵌入的PDF。

```
router.get('/job/:id', async function(req, res, next) {
    const id = req.params.id;

    const pgClient = new Client();
    pgClient.connect();

    const pgResult = await pgClient.query("SELECT name FROM job_postings WHERE id =
  $1", [id]);
    const name = pgResult.rows[0].name;

    res.render('job', { pdf_url: `/pdf/${id}`, name });
  });
```

在views/目錄中，建立包含以下內容的job.jade檔案：

```
  extends layout

  block content
    h1= name
    div(id='adobe-dc-view')
    script(src='https://documentcloud.adobe.com/view-sdk/main.js')
    script.
      window.embedUrl = "!{pdf_url}";
    script(src='/javascripts/embed-pdf.js')
```

第一個指令碼是Adobe的View SDK，它使嵌入PDF變得容易。 第二個指令碼是一個串聯單行，它將window.embedUrl的值設定為Express路由處理程式提供的PDF的URL。 自行建立第三個指令碼，如下所示：

```
  document.addEventListener("adobe_dc_view_sdk.ready", function () {
    var adobeDCView = new AdobeDC.View({ clientId: "YOUR API KEY HERE", divId:
   "adobe-dc-view" });
    adobeDCView.previewFile({
      content: { location: { url: '//' + window.location.host + window.embedUrl }
         },
      metaData: { fileName: "Job posting" }
    });
  });
```

現在，您可以測試上載文檔、重定向到/job/id頁以及查看嵌入PDF的整個過程。 您的用戶將通過相同步驟將職務發佈或其他文檔添加到您的網站。

![測試已上載PDF文檔的螢幕快照](assets/jobs_2.png)

若要查看操作中的內嵌，請查看此[即時演示](https://documentcloud.adobe.com/view-sdk-demo/index.html#/view/IN_LINE/Bodea%20Brochure.pdf)。

## 後續步驟

本操作教程詳細介紹了如何將Node.js與[!DNL Acrobat Services]一起使用，將以各種格式上傳的[作業發佈](https://developer.adobe.com/document-services/use-cases/content-publishing/job-posting)轉換為PDF。 隨後，生成的PDF被嵌入到網頁中。 現在，您可以將相同的功能添加到您的網站中，使雇主能夠更輕鬆地上傳工作說明、手冊等，供求職者查找。 這些能力可以幫助每個人獲得找到夢寐以求的工作所需的資訊。

[!DNL Acrobat Services]幫助您將密鑰文檔處理功能添加到網站或應用。 如果您想更深入地瞭解這些API可以做什麼，請參閱以下快速入門文檔：

* [PDF嵌入API](https://www.adobe.com/devnet-docs/dcsdk_io/viewSDK/index.html)

* [PDF服務API](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html)

若要開始向網站添加用戶友好的文檔處理功能，請[註冊免費試用版](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html)。 Adobe PDF嵌入式API始終是免費使用的，Adobe PDF服務API是6個月免費的，而且每個文檔交易只有\$0.05，因此您可以隨著業務增長[按需付費](https://developer.adobe.com/document-services/pricing/main)。

