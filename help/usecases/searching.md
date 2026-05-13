---
title: 搜尋與索引
description: 學習如何從掃描文件建立可搜尋的 PDF 檔案
feature: Use Cases
role: Developer
level: Intermediate
type: Tutorial
jira: KT-8095
thumbnail: KT-8095.jpg
exl-id: a22230b5-1ff2-4870-84da-f06a904c99e1
TQID: https://experienceleague.adobe.com/ceJsh2lv-S4b6mScT7DF83x1C85BAu-bPa5ju4JcYu4
product_v2: id: acdc2bde-2937-4877-90d9-031dd66278c9
feature_v2: id: b1809bd0-a86b-4991-8083-2e3b517fc3b8id: c4d07275-6387-4756-8bf7-681e581ffd27
subfeature_v2: id: b4b3dc0f-b1be-46b4-b8ca-134a4629084aid: c4b1e8f2-d9a8-4792-b5e4-be52bd870028id: c6f72a9c-54c4-4933-93c9-d7c656ff1f14
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2: id: b5a62a22-46f7-4f0d-b151-3fc640bef588
topic_v2: id: bce87dde-a4ab-44c9-8a18-ad66e4ddb377
source-git-commit: 0110d2606056220c4236fe2f0e3afbfc112746e7
workflow-type: tm+mt
source-wordcount: 1389
ht-degree: 0%

---

# 搜尋與索引

![使用案例英雄卡池](assets/UseCaseSearchingHero.jpg)

組織經常必須將紙本文件和掃描檔案數位化。 請想像這個 [情境](https://docs.google.com/document/d/11jZdVQAw-3fyE3Y-sIqFFTlZ4m02LsCC/edit)。 一家律師事務所掃描了數千份法律合約以建立數位檔案。 他們想確認這些法律合約中是否有特定條款或補充條款需要修訂。 準確性對合規至關重要。 解決方案是盤點數位文件，讓文本可搜尋，並建立索引以查找這些資訊。

建立數位檔案庫以檢索資訊以供編輯或後續作業的挑戰，對大多數組織來說都是一場惡夢。

## 你可以學到什麼

這本實作教學將探討 API 的功能以及如何 [!DNL Adobe Acrobat Services] 輕鬆用於文件的歸檔與數位化。 你透過建立 Express NodeJS 應用程式來探索這些功能，然後整合 [!DNL Acrobat Services] 用於歸檔、數位化和文件轉換的 API。

要跟上，你需要 [安裝Node.js](https://nodejs.org/) ，並且對 Node.js 和 [ES6 語法](https://www.w3schools.com/js/js_es6.asp)有基本了解。

## 相關 API 與資源

* [PDF 服務 API](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/index.html)

* [專案程式碼](https://github.com/agavitalis/AdobeDocumentServicesAPIs.git)

## 專案設定

首先，為應用程式設定資料夾結構。 你可以在這裡](https://github.com/agavitalis/AdobeDocumentAPI.git)取得原始碼[。

## 目錄結構

建立一個名為 AdobeDocumentServicesAPIs 的資料夾，並在你選擇的編輯器中開啟。 使用以下資料夾結構建立一個基本的 NodeJS 應用程式 `npm init` ：

```
AdobeDocumentServicesAPIs
config
default.json
controllers
createPDFController.js
makeOCRController.js
searchController.js
models
document.js
output
.gitkeep
routes
web.js
services
upload.js
views
index.hbs
ocr.hbs
search.hbs
index.js
```

你使用 MongoDB 作為這個應用程式的資料庫。 因此，設定時，將預設的資料庫設定放入 config/資料夾，方法是將下方的程式碼片段貼到該資料夾的 default.json 檔，然後加入資料庫的 URL。

```
### config/default.json and config/dev.json
{ "DBHost": "YOUR_DB_URI" }
```

## 套件安裝

現在，請使用以下程式碼片段所示的 npm 安裝指令來安裝一些套件：

```
{
    "name": "adobedocumentservicesapis",
    "version": "1.0.0",
    "description": "",
    "main": "index.js",
    "directories": {
    "test": "test"
    },
    "dependencies": {
    "body-parser": "^1.19.0",
    "config": "^3.3.6",
    "express": "^4.17.1",
    "hbs": "^4.1.1",
    "mongoose": "^5.12.1",
    "morgan": "^1.10.0",
    "multer": "^1.4.2",
    "path": "^0.12.7"
    },
    "devDependencies": {},
    "scripts": {
    "start": "set NODE_ENV=dev && node index.js"
    },
    "repository": {
    "type": "git",
    "url": "git+https://github.com/agavitalis/AdobeDocumentServicesAPIs.git"
    },
    "author": "Ogbonna Vitalis",
    "license": "ISC",
    "bugs": {
    "url": "https://github.com/agavitalis/AdobeDocumentServicesAPIs/issues"
    },
    "homepage": "https://github.com/agavitalis/AdobeDocumentServicesAPIs#readme"
}
```

```
###bash
npm install express mongoose config body-parser morgan multer hbs path pdf-parse
Ensure that the content of your package.json file is similar to this code snippet:
###package.json
{
```

這些程式碼片段安裝應用程式相依關係，包括檢視的 Handlebars 模板引擎。 在腳本標籤中，你可以設定應用程式的執行時參數。

## 整合 [!DNL Acrobat Services] API

[!DNL Acrobat Services] 包含三個 API：

* Adobe PDF 服務 API

* Adobe PDF 嵌入 API

* Adobe 文件產生 API

這些 API 透過一組雲端網路服務自動化產生、操作及轉換 PDF 內容。

要取得憑證，你需要註冊[](https://www.adobe.com/go/dcsdks_credentials?ref=getStartedWithServicesSDK)並完成整個工作流程。PDF 嵌入 API 是免費使用的。 PDF 服務 API 與文件產生 API 免費提供六個月。 試用期結束後，您可以 [以每筆文件交易僅需 0.05 美元的價格按需](https://developer.adobe.com/document-services/pricing/main) 付費。 你只有在公司成長和處理更多合約時才會付費。

![建立憑證的截圖](assets/searching_1.png)

完成註冊後，會下載包含 API 憑證的程式碼範例到你的電腦。 將此程式碼範例解壓，並將private.key與pdftools-api-credentials.json檔案置於應用程式的根目錄。

現在，透過應用程式根目錄中的終端機執行` npm install --save @adobe/documentservices-pdftools-node-sdk `指令，安裝 [PDF Services Node.js SDK](https://www.npmjs.com/package/@adobe/documentservices-pdftools-node-sdk)。

## 建立 PDF

[!DNL Acrobat Services] 支援從 Microsoft Office 文件（Word、Excel 和 PowerPoint）及其他 [支援的檔案格式](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/howtos.html#create-a-pdf) （如 .txt、.rtf、.bmp、.jpg、.gif、.tiff 和 .png 建立 PDF。

若要從支援的檔案格式建立 PDF 文件，請使用此表單上傳文件。 你可以在 [GitHub](https://github.com/agavitalis/AdobeDocumentServicesAPIs.git) 上取得表單的 HTML 和 CSS 檔案。

![網頁表單截圖](assets/searching_2.png)

現在，將以下程式碼片段加入控制器/createPDFController.js檔案。 此程式碼會擷取文件並將其轉換成 PDF。

原始檔案和轉換後的檔案會儲存在應用程式內的資料夾中。

```
const PDFToolsSdk = require('@adobe/documentservices-pdftools-node-sdk');
/*
* GET / route to show the createPDF form.
*/
function createPDF(req, res) {
//catch any response on the url
let response = req.query.response
res.render('index', { response })
}
/*
* POST /createPDF to create a new PDF File.
*/
function createPDFPost(req, res) {
let filePath = req.file.path;
let fileName = req.file.filename;
try {
// Initial setup, create credentials instance.
const credentials = PDFToolsSdk.Credentials
.serviceAccountCredentialsBuilder()
.fromFile("pdftools-api-credentials.json")
.build();
// Create an ExecutionContext using credentials and create a new operation
instance.
const executionContext = PDFToolsSdk.ExecutionContext.create(credentials),
createPdfOperation = PDFToolsSdk.CreatePDF.Operation.createNew();
// Set operation input from a source file.
const input = PDFToolsSdk.FileRef.createFromLocalFile(filePath);
createPdfOperation.setInput(input);
// Execute the operation and Save the result to the specified location.
createPdfOperation.execute(executionContext)
.then((result) => {
result.saveAsFile('output/createPDFFromDOCX.pdf')
//download the file
res.redirect('/?response=PDF Successfully created')
})
.catch(err => {
if (err instanceof PDFToolsSdk.Error.ServiceApiError
|| err instanceof PDFToolsSdk.Error.ServiceUsageError) {
console.log('Exception encountered while executing operation',
err);
} else {
console.log('Exception encountered while executing operation',
err);
}
});
} catch (err) {
console.log('Exception encountered while executing operation', err);
}
}
//export all the functions
module.exports = { createPDF, createPDFPost };
```

此程式碼片段需要 [PDF 服務Node.js SDK](https://www.npmjs.com/package/@adobe/documentservices-pdftools-node-sdk) 服務。 使用以下函數：

* createPDF，顯示上傳文件表單

* createPDFPost，將上傳的文件轉換成 PDF

轉換後的 PDF 文件會儲存在輸出目錄，而原始檔案則儲存在上傳目錄。

## 使用文字辨識

光學字元辨識（OCR）將影像和掃描文件轉換為可搜尋的檔案。 你可以將 API、圖片和掃描文件轉換 [!DNL Acrobat Services] 成可搜尋的 PDF。 完成 OCR 操作後，檔案即可編輯與搜尋。 你可以將檔案內容儲存在資料庫中，方便索引或其他用途。

回顧掃描文件的搜尋與索引對於許多組織來說至關重要，因為檔案管理與資訊處理至關重要。 OCR 功能消除了這些挑戰。

要實現此功能，您必須設計一個類似上述的上傳表單。 這次，你限制表單只能使用 PDF 檔案，因為 OCR 功能只能在 PDF 文件上使用。

這是這個範例的上傳表單：

![上傳檔案的表單截圖](assets/searching_3.png)

現在，要操作上傳的 PDF 並執行一些 OCR 操作，請將下方的程式碼片段加入控制器/makeOCRController.js檔案。 此程式碼會在上傳的檔案上實作 OCR 處理，並將該檔案儲存在應用程式的檔案系統中。

```
const fs = require('fs')
const pdf = require('pdf-parse');
const mongoose = require('mongoose');
const Document = require('../models/document');
const PDFToolsSdk = require('@adobe/documentservices-pdftools-node-sdk');
/*
* GET /makeOCR route to show the makeOCR form.
*/
function makeOCR(req, res) {
//catch any response on the url
let response = req.query.response
res.render('ocr', { response })
}
/*
* POST /makeOCRPost to create a new PDF File.
*/
function makeOCRPost(req, res) {
let filePath = req.file.path;
let fileName = req.file.filename;
try {
// Initial setup, create credentials instance.
const credentials = PDFToolsSdk.Credentials
.serviceAccountCredentialsBuilder()
.fromFile("pdftools-api-credentials.json")
.build();
// Create an ExecutionContext using credentials and create a new operation
instance.
const executionContext = PDFToolsSdk.ExecutionContext.create(credentials),
ocrOperation = PDFToolsSdk.OCR.Operation.createNew();
// Set operation input from a source file.
const input = PDFToolsSdk.FileRef.createFromLocalFile(filePath);
ocrOperation.setInput(input);
// Execute the operation and Save the result to the specified location.
ocrOperation.execute(executionContext)
.then(async (result) => {
let newFileName = `createPDFFromDOCX-${Math.random() * 171}.pdf`;
await result.saveAsFile(`output/${newFileName}`);
let documentContent = fs.readFileSync(
require("path").resolve("./") + `\\output\\${newFileName}`
);
pdf(documentContent)
.then(function (data) {
//Creates a new document
var newDocument = new Document({
documentName: fileName,
documentDescription: description,
documentContent: data.text,
url: require("path").resolve("./") + `\\output\\${newFileName}`
});
//Save it into the DB.
newDocument.save((err, docs) => {
if (err) {
res.send(err);
} else {
//If no errors, send it back to the client
res.redirect(
"/makeOCR?response=OCR Operation Successfully performed on
the PDF File"
);
}
});
})
.catch(function (error) {
// handle exceptions
console.log(error);
});
})
.catch(err => {
if (err instanceof PDFToolsSdk.Error.ServiceApiError
|| err instanceof PDFToolsSdk.Error.ServiceUsageError) {
console.log('Exception encountered while executing operation',
err);
} else {
console.log('Exception encountered while executing operation',
err);
}
});
} catch (err) {
console.log('Exception encountered while executing operation', err);
}
}
//export all the functions
module.exports = { makeOCR, makeOCRPost };
```

你需要 [!DNL Acrobat Services] Node SDK、mongoose、pdf-parse 和 fs 模組，以及你的文件模型架構。 這些模組是將轉換後檔案內容儲存到 MongoDB 資料庫所必需的。

現在建立兩個函式：makeOCR 顯示上傳的表單，然後 makeOCRPost 處理上傳的文件。 先把原始表單存到資料庫，然後把轉換後的表單存到應用程式的輸出資料夾。

Adobe 提供的憑證會從 pdftools-api-credentials.json 檔案中載入，然後再轉換檔案。

>[!NOTE]
>
>OCR 功能僅支援 PDF 文件。

另外，請將下方的程式碼片段加入應用程式的 Modes/Document.js 檔案中。

在程式碼片段中，定義一個貓鼬模型，然後描述文件中要儲存在資料庫中的屬性。 另外，請索引 documentContent 欄位，讓搜尋文字變得更簡單且有效率。

```
const mongoose = require("mongoose");
const Schema = mongoose.Schema;
//Document schema definition
var DocumentSchema = new Schema(
{
documentName: { type: String, required: false },
documentDescription: { type: String, required: false },
documentContent: { type: String, required: false },
url: { type: String, required: false },
status: {
type: String,
enum : ["active","inactive"],
default: "active"
}
},
{ timestamps: true }
);
//for text search
DocumentSchema.index({
documentContent: "text",
});
//Exports the DocumentSchema for use elsewhere.
module.exports = mongoose.model("document", DocumentSchema);
```

## 搜尋文本

現在你實作一個簡單的搜尋功能，讓使用者能進行一些簡單的文字搜尋。 你也會新增下載功能，讓 PDF 檔案能夠被下載。

此功能需要簡單的表單和卡片來顯示搜尋結果。 你可以在 [GitHub](https://github.com/agavitalis/AdobeDocumentServicesAPIs.git) 找到表單和卡片的設計。

下方截圖展示了搜尋功能及搜尋結果。 你可以下載任何搜尋結果。

![搜尋功能截圖](assets/searching_4.png)

要實作搜尋功能，請在應用程式的控制器資料夾中建立一個searchController.js檔案，並貼上以下程式碼片段：

```
const fs = require('fs')
const mongoose = require('mongoose');
const Document = require('../models/document');
/*
* GET / route to show the search form.
*/
function search(req, res) {
//catch any response on the url
let response = req.query.response
res.render('search', { response })
}
/*
* POST /searchPost to search the contents of your saved file.
*/
function searchPost(req, res) {
let searchString = req.body.searchString;
Document.aggregate([
{ $match: { $text: { $search: searchString } } },
{ $sort: { score: { $meta: "textScore" } } },
])
.then(function (documents) {
res.render('search', { documents })
})
.catch(function (error) {
let response = error
res.render('search', { response })
});
}
//export all the functions
module.exports = { search, searchPost, downloadPDF };
```

現在實作下載功能，讓使用者能下載從搜尋中回傳的文件。

## 下載文件

實作下載功能與你已經做過的類似。 請在controllers/earchController.js檔案的搜尋後，新增以下程式碼片段：

```
/*
* POST /downloadPDF To Download PDF Documents.
*/
async function downloadPDF(req, res) {
console.log("here")
let documentId = req.params.documentId
let document = await Document.findOne({_id:documentId});
res.download(download.link);
}
```

## 後續步驟

在這個實作教學中，你將 API 整合 [!DNL Acrobat Services] 進一個Node.js應用程式，並利用該 API 實現文件轉換功能，將檔案轉換成 PDF。 你新增了OCR功能，讓照片和掃描檔案都能被搜尋。 然後，你把檔案存到資料夾裡，方便下載。

接著，你新增了搜尋功能，可以搜尋用 OCR 轉換成文字的文件。 最後，你實作了下載功能，讓這些檔案更容易下載。 您完成的申請表讓法律事務所更容易找到並處理特定文字。

[!DNL Acrobat Services]由於其韌性與易用性，與其他服務相比，使用文件轉換非常推薦。你可以快速建立帳號，開始享受 API 在文件轉換與管理方面的功能 [!DNL Acrobat Services] 。

現在你已經對如何使用 [!DNL Acrobat Services] API 有了紮實的理解，可以透過練習來提升你的技能。 你可以複製這個教學中使用的資料庫，並嘗試剛學到的一些技能。 更好的是，你可以嘗試在探索 API 無限可能性 [!DNL Acrobat Services] 的同時重建這個應用程式。

準備好在自己的應用程式中啟用文件共享與審查了嗎？ 註冊你的 [[!DNL Adobe Acrobat Services]](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html)開發者帳號。 享受六個月免費試用，隨著 [業務成長，每筆文件交易只需 \0.05 的即用](https://developer.adobe.com/document-services/pricing/main) 付費。
