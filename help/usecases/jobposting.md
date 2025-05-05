---
title: 刊登工作貼職
description: 瞭解如何為求職者和僱主開發順暢且一致的網頁體驗
feature: Use Cases
role: Developer
level: Intermediate
type: Tutorial
jira: KT-8092
thumbnail: KT-8092.jpg
exl-id: 0e24c8fd-7fda-452c-96f9-1e7ab1e06922
source-git-commit: c6272ee4ec33f89f5db27023d78d1f08005b04ef
workflow-type: tm+mt
source-wordcount: '1447'
ht-degree: 0%

---

# 張貼工作

![使用案例主打橫幅](assets/UseCaseJobHero.jpg)

當使用多個用戶的網站時，設計確保人人享有順暢體驗的體驗至關重要。

想像下列情況：您有一個網站可讓 [僱主上傳工作貼文](https://developer.adobe.com/document-services/use-cases/content-publishing/job-posting)。 對於求職者，可以輕鬆地以一致的格式輕鬆檢視發文相關的所有檔。 但是，僱主可以方便地以任何發生的檔格式附加資訊。 為了為這兩種類型的使用者提供便利，您可以自動將所有已上傳的文件轉換為 PDF，並將其內嵌到張貼中。

## 您可以學習哪些內容

此實作教學課程逐步介紹使用 [!DNL Adobe Acrobat Services] SDK [及其Node.js SDK](https://www.npmjs.com/package/@adobe/documentservices-pdftools-node-sdk) 將這些功能新增至工作張貼網站的Node.js範例。 這會建立網站，方便使用，而且對求職者和求職者都更具吸引力。 以下是[完整的](https://github.com/contentlab-io/adobe_job_posting) [項目代碼](https://github.com/contentlab-io/adobe_job_posting)，以防您在閱讀時遵循。

首先，請設定簡單的 Express 架構Node.js網頁應用程式。 [Express](https://expressjs.com/) 是極簡主義的網頁應用程式架構，提供路由和範本等功能。 應用程式的程式代碼可在 GitHub 上 [使用](https://github.com/contentlab-io/adobe_job_posting)。 此外，請安裝 [PostgreSQL 資料庫](https://www.postgresql.org/) 並將其設定為儲存 PDF。

## 相關 [!DNL Acrobat Services] API

* [PDF 嵌入API](https://www.adobe.com/devnet-docs/dcsdk_io/viewSDK/index.html)

* [PDF 服務API](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html)

## 建立Adobe API認證

首先，您必須[建立Adobe PDF嵌入API的認證](https://www.adobe.com/go/dcsdks_credentials) （免費使用） 和 Adobe PDF Services API （6 個月免費，然後[&#128279;](https://developer.adobe.com/document-services/pricing/main)按您付費，每份檔交易只要 \$0.05）。API建立 PDF Services 認證時，請選取「建立個人化程式代碼範例」選項。 儲存 ZIP 檔案並解壓縮 pdftools-api-credentials.json，然後將private.key到 Node.js Express 專案的根目錄。

您也需要可自由使用的內嵌API API鍵。 從 [專案](https://developer.adobe.com/console/projects)，移至您建立的專案。 然後，按一下 **「新增至專案」** ，然後選取 **「API」**。 最後，按兩下 **「PDF 嵌入」API**。

指定 PDF 內嵌API的網域。 API機碼必須公開 （在瀏覽器所執行的程式代碼中尋找）。 指定網域可確保其他網域中的其他人無法使用API鍵。

您無法將「localhost」當做網域使用。 指定網域 （例如「testing.local」），然後編輯電腦上的 hosts 檔案，將該網域重新導向至 127.0.0.1您的電腦。 然後，您可以在localhost：3000測試應用程式，而不必在test.local：3000進行測試。 完成後，請在項目頁面上找到 PDF 內嵌API API鍵。

## 新增上傳表單和處理程式

使用使用中的 Express 應用程式和 API 認證時，您還需要一個表單，讓使用者能夠將檔案上傳至網站。 針對此目的編輯 index.jade 範本。

為上傳的工作發文名稱和包含詳細資訊的檔建立輸入欄位。

在範本的內容區塊內，新增下列表單：

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

接下來，為 /上傳動作新增POST要求的處理程式。 然後，為路由/上傳至路由/index.js檔案新增路由。 您可以為此路徑建立新檔案，但必須更新 app.js 檔案以反映新檔案。 您可以在此路由處理程式中存取指定的名稱和上傳的檔案。

```
router.post('/upload', async function (req, res, next) {
    const name = req.body.name;
    const fileContents = req.files.attachment.data;

    // code to work with the uploaded document
  });
```

此功能是異步的，因此您可以在功能中使用等候中的關鍵詞，這對於呼叫執行API呼叫的方法很方便。

![工作張貼網站的螢幕擷圖](assets/jobs_1.png)

## 使用 PDF 服務API

在使用 PDF 服務 API 之前，您必須將下列讀入專案新增至路由檔案頂端：

```
const PDFToolsSdk = require('@adobe/documentservices-pdftools-node-sdk');
  const { Readable } = require('stream');
```

直接在匯入中，您可以載入API認證並建立 [執行內容](https://www.javascripttutorial.net/javascript-execution-context/)。 由於您可以針對不同的作重複使用執行情境，因此只做一次是有意義的作。

```
  const credentials = PDFToolsSdk.Credentials
  .serviceAccountCredentialsBuilder()
  .fromFile("pdftools-api-credentials.json")
  .build();

  const executionContext = PDFToolsSdk.ExecutionContext.create(credentials);
```

現在，請回到區塊中註釋的請求處理程式中 `router.post` 撰寫程序代碼。 首先，將文件轉換為 PDF。

```
  const createPdfOperation = PDFToolsSdk.CreatePDF.Operation.createNew();

  const input = PDFToolsSdk.FileRef.createFromStream(Readable.from(fileContents),
  req.files.attachment.mimetype);

  createPdfOperation.setInput(input);

  let result = await createPdfOperation.execute(executionContext);

  result.saveAsFile('output-pdf' + new Date().getTime() + '.pdf');
  return res.send('success!');
```

大部分作業都採用相同的四個步驟。 首先，使用適當類別的建立新方法初始化作業類型。 然後，建立作的輸入，也就是 FileRef。 後續作業可略過此步驟，因為作業結果也是 FileRef。 對於此初始作，請從上傳檔案的位元組建立 FileRef。 第三，您必須將輸入指派給作。 最後，作會執行，執行內容會當做執行方法中的參數。 此方法會傳回「承諾」，讓您等著結果。

程式代碼會將傳回的 PDF 儲存至檔案，並傳送簡單的「成功」回應至瀏覽器。 檔名的「日期」部分會保證唯一的檔名。 如果目的地檔案存在，saveAsFile 會傳回錯誤。

## 將影像轉換為文字並壓縮 PDF

現在，使用光學字元識別 （OCR） 將影像轉換為文字，然後壓縮結果。 您可以使用類似於 CreatePDF作的 OCR 和 CompressPDF作來進行此作。 將下列專案新增至路由檔案中，位於 `router.post`：

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

僅需執行此作一次，因為其結果為 FileRef，程式代碼可傳遞至 setInput。

有比將檔案儲存在硬碟上並返回過度簡化的 HTTP 回應更好的替代方案。 請改為將 PDF 儲存在資料庫中，並顯示使用 Adobe 的免費 PDF 內嵌API嵌入 PDF 的網頁。 如此一來，僱主的求職貼文或手冊便會顯示在網站上，供求職者尋找和檢視，並包含公司標誌和其他設計元素。

## 將 PDF 儲存在資料庫中

將 PDF 儲存在 PostgreSQL 資料庫中。 取得節點 -postgres 套件，即可在 Node.js 中聯機至 Postgres。 安裝串流緩衝區套件，因為在某些時候，您必須將 PDF 內容儲存在緩衝區中，而 FileRef 僅適用於串流。 因此，請使用串流緩衝套件將內容寫入緩衝區。

```
npm install pg stream-buffers
```

現在，建立一個工作發件的資料庫表格。 它需要唯一標識碼的欄、名稱的欄，以及附加 PDF 的欄。 您可以從 Postgres 命令列介面 （CLI） 建立資料庫表格：

```
CREATE TABLE job_postings (id TEXT PRIMARY KEY, name TEXT NOT NULL, attachment
BYTEA NOT NULL);
```

返回Node.js檔案。 在檔案頂端新增一些匯入：

```
  const { Client } = require('pg');
  const streamBuffers = require('stream-buffers');
```

若要將 PDF 儲存在資料庫表格中，請修改上傳功能。 將最後兩行 （saveAsFile 並傳送） 取代為此代碼段：

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

若要寫入內容，請建立可寫入的StreamBuffer。 在完成事件后，就可以執行 SQL 查詢了。 節點-postgres 套件會自動將緩衝區參數轉換為 BYTEA 格式。 查詢會將使用者重新導向 /job/{id}，這是稍後建立的端點。

對於 PDF 內嵌 API，您還需要傳回 PDF 內容的端點：

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

## 嵌入 PDF

現在建立 /job/{id} 端點，這會顯示包含所要求的工作發文名稱和內嵌 PDF 的範本。

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

在檢視/目錄中，使用此內容建立job.jade 檔案：

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

第一個腳本是Adobe的檢視 SDK，可讓您輕鬆嵌入 PDF。 第二個腳本是行內單行，可將 window.embedUrl 的值設定為 Express 路由處理程式提供的 PDF URL。 請依照下列方式自行建立第三個腳本：

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

現在，您可以測試上傳檔、重新導向至 /job/id 頁面以及檢視內嵌 PDF 的整個程式。 您的用戶會執行相同的步驟，將工作貼文或其他檔新增至您的網站。

![測試已上傳 PDF 檔的螢幕擷圖](assets/jobs_2.png)

若要查看嵌入實況，請查看此 [實時示範](https://documentcloud.adobe.com/view-sdk-demo/index.html#/view/IN_LINE/Bodea%20Brochure.pdf)。

## 後續步驟

這個實作教學課程介紹如何使用 Node.js [!DNL Acrobat Services] 來將上傳 [的發](https://developer.adobe.com/document-services/use-cases/content-publishing/job-posting) 文轉換為 PDF 的不同格式。 產生的 PDF 接著會嵌入網頁中。 現在您可以將相同的功能新增至您的網站，讓僱主更容易上傳工作說明、手冊等，以便求職者尋找。 這些功能可協助每個人找到找到理想工作所需的資訊。

[!DNL Acrobat Services] 協助您新增關鍵檔處理功能至您的網站或應用程式。 如果您想深入瞭解這些 API 的功能，請參閱以下快速入端檔：

* [PDF 嵌入API](https://www.adobe.com/devnet-docs/dcsdk_io/viewSDK/index.html)

* [PDF 服務API](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html)

若要開始將方便使用的文件處理功能新增至您的網站， [請註冊免費試用版](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html)。 Adobe PDF內嵌API一律免費使用，Adobe PDF Services API為六個月免費，每筆檔交易只要 \$0.05，因此您可以 [隨著業務成長隨時隨地付費](https://developer.adobe.com/document-services/pricing/main) 。
