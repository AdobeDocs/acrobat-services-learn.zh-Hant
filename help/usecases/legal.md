---
title: 管理法律合約
description: 瞭解如何使用自定義數據輸入自動產生和保護法律檔
feature: Use Cases
role: Developer
level: Intermediate
type: Tutorial
jira: KT-8097
thumbnail: KT-8097.jpg
exl-id: e0c32082-4f8f-4d8b-ab12-55d95b5974c5
source-git-commit: c6272ee4ec33f89f5db27023d78d1f08005b04ef
workflow-type: tm+mt
source-wordcount: '1890'
ht-degree: 0%

---

# 管理法律合約

![使用案例主打橫幅](assets/UseCaseLegalHero.jpg)

數位化帶來挑戰。 現在，大多數組織有許多類型的 [法律合約](https://developer.adobe.com/document-services/use-cases/agreements-and-contracts/legal-contracts) ，他們必須建立、編輯、核准以及由不同方簽署。 這些法律合約通常需要獨一無二的自定義和品牌化。 組織可能也會在完成簽署后，將其儲存為受保護的格式，以確保安全。 要完成所有這些工作，他們需要強大的檔產生和管理解決方案。

許多解決方案提供一些檔產生方式，但無法自定義數據輸入和條件式邏輯，例如僅適用於特定情況的條款。 隨著文件越來越廣泛，手動更新公司的法務範本十分具挑戰性且容易出錯。 自動執行這些程序相當需要。

## 您可以學習哪些內容

在此實作教學課程中，探索在檔中產生自定義輸入欄位中的 API[&#128279;](https://developer.adobe.com/document-services/apis/doc-generation) 功能[!DNL Adobe Acrobat Services] 。同時，探索如何輕鬆將這些產生的文件轉換為受保護的可攜式檔格式 （PDF），以防止數據處理。

探索將合約轉換為 PDF 時，本教學課程包含一些程序設計。 若要有效Microsoft作， [請在計算機上安裝 Microsoft Word](https://www.microsoft.com/en-us/download/office.aspx) 和 [Node.js](https://nodejs.org/) 。 也建議您基本瞭解Node.js和 [ES6語法](https://www.w3schools.com/js/js_es6.asp) 。

## 相關 API 和資源

* [Adobe檔產生API](https://developer.adobe.com/document-services/apis/doc-generation)

* [PDF 嵌入API](https://www.adobe.com/devnet-docs/dcsdk_io/viewSDK/index.html)

* [Adobe Sign API](https://developer.adobe.com/adobesign-api/)

* [項目代碼](https://github.com/agavitalis/adobe_legal_contracts.git)

## 建立範本檔

您可以使用 Microsoft Word 應用程式或下載Adobe的 [Word 範](https://developer.adobe.com/document-services/apis/doc-generation#sample-blade)例來建立法律檔。 不過，不需使用 Adobe Document Generation Tagger 載[&#128279;](https://developer.adobe.com/document-services/docs/overview/document-generation-api/wordaddin)入巨集 （針對 Microsoft Word） 使用輔助工具，就不容易自定義輸入內容並進行數字簽署。

Document Generation Tagger 是 Microsoft Word 載入巨集，專為使用標籤使檔自定義作業順暢進行。 其可在文件範本中建立動態字段，並以 JSON 數據動態填滿。

![如何在 Word 中新增 Adoe Document Generation Tagger 的螢幕擷圖](assets/legal_1.png)

若要說明 Document Generation Tagger 的使用方式，請安裝此載入巨集，然後建立 JSON 數據模型，該模型用於簡單的法律合約文件的標記。

按兩下「 **插入** 」索引標籤，在 Word 中安裝 Document Generation Tagger，然後在「載入巨集」群組中按一下 **「我的載入巨集」**。 在「Office 載入巨集」選單中，搜尋「Adobe檔產生」，然後按兩下「 **新增** 」並依照程序進行。 您可以在上方的螢幕擷取中看到這些步驟。

安裝適用於 Word 載入巨集的 Document Generation Tagger 後，建立簡單的 JSON 數據模型來標記法律檔。

若要繼續進行，請開啟您選擇的任何編輯器，建立一個名為「Agreement.json」的檔案，然後將代碼段貼到您建立的 JSON 檔案中。

```
{
"Agreement": {
"Date": "1/24/2021",
"Prime Contractor Name": "Ogbonna Vitalis Corp",
"Prime State": "Lagos",
"Address": "Maryland Ave, Lagos State, Ng",
"Sub Contractor Name": "Vivvaa Soln",
"Sub Contractor State": "California",
"Sub Contractor Address": "Molusi Avenue, Dallas Texas, CA",
"Agreement Date": "1/24/2021",
"Length": 5
}
}
```

儲存此 JSON 檔後，將其匯入至 Document Generation Tagger 載入巨集。 在 Word 畫面右上方的Adobe群組中按一下 **「文件產生** 」，以讀入檔，如下方螢幕擷取所示。

![Word 中 Adobe Document Generation Tagger 載入巨集的螢幕擷圖](assets/legal_2.png)

這會顯示引導您的影片。 您可以觀看或直接前往標記字段，方法是按兩下 **開始使用**。 按兩下開始使用&#x200B;**&#x200B;**&#x200B;後，上傳窗體會隨即顯示。單擊 **「上傳 JSON 檔案** 」，然後選取您剛建立的 JSON 檔案。 讀入完成後，按兩下 **「產生標籤** 」產生標籤。

在匯入併產生標籤后，您可以將這些標籤新增至檔。 若要新增游標，請將游標放在您希望標籤出現的確切位置。 然後從「文件產生」標籤API，然後按下「 **插入文字」**。 下方的螢幕擷取會概述此程式。

![將標籤新增至文件的螢幕擷圖](assets/legal_3.png)

除了使用匯入的 JSON 數據模型建立的基本標籤之外，您還可以使用進階功能進行更多選項，例如影像、條件式邏輯、計算、重複元素以及條件式詞組。 您可以按下 **「文件產生記錄器」面板中的「進** 階」，存取這些功能。 您可以在下方的螢幕擷取中看到此內容。

![Adobe Document Generation Tagger 進階卷標的螢幕擷圖](assets/legal_4.png)

這些進階功能與基本標籤並無不同。 若要包含條件式邏輯，請選取要填寫的檔部分。 然後，設定決定標籤插入的規則。

若要進一步說明，假設合約中有一個您想要包含的區段，只有條件式。 在「選取內容類型」欄位中，選取「 **區段」。** 在「選取記錄」欄位中，選擇決定是否顯示條件式區段的選項。 選取您需要的條件式運算符，然後設定您在「值」欄位中所測試的值。 然後按下「 **插入條件」。** 下方的螢幕擷取會說明此過程。

![插入條件式內容的螢幕擷圖](assets/legal_5.png)

在計算中，選取「Arithmetic」或「匯總」，然後包含相關的第一個記錄、運算符和第二個記錄，以根據可用的範本標籤使用。 然後按下「 **插入計算」**。

此外，法律合約通常需要參與方的簽名。 您可以使用「數字計算」區段下方的Adobe Sign文字標籤來插入電子簽名。 若要加入電子簽名，您必須據此從下拉式清單中指定收件者數量、選 **取「簽署者**」和字段類型。 完成後，按下 **「插入Adobe Sign文字標籤** 」以完成程式。

為確保數據完整性，請以受保護的格式儲存法律檔。 透過 [!DNL Acrobat Services] API，您可以快速地將檔案轉換為 PDF 格式。 您可以建立簡單的快速Node.js應用程式，將檔產生API整合，並使用這個簡單的應用程式將標籤檔從 Word 轉換為 PDF 格式。

## 項目設定

首先，設定 Node.js 應用程式的檔案夾結構。 在此範例中，請呼叫這個簡單的應用程式 AdobeLegalContractAPI。 您可以在這裡[&#128279;](https://github.com/agavitalis/adobe_legal_contracts.git)擷取源碼。

### 目錄結構

建立一個名為 AdobeLegalContractAPI 的檔案夾，然後在您選擇的編輯器中開啟。 使用下列資料夾結構， ```npm init``` 使用指令建立基本Node.js應用程式：

```
###Directory Structure
AdobeLegalContractAPI
-----config
----------default.json
-----controllers
----------createPDFController.js
----------previewController.js
-----models
----------document.js
-----routes
----------web.js
-----services
-----------upload.js
-----uploads
-----views
-----index.js
```

以上為適用於您應用程式的簡單Node.js應用程式結構。 現在請繼續安裝必要的 npm 套件。

### 套件安裝

請使用 npm 安裝命令安裝所需的套件，如下方代碼段所示：

```
npm install express body-parser morgan multer hbs path config mongoose
```

安裝套件後，請確認您package.json檔案的內容與下列代碼段相同：

```
###package.json
{
"name": "adobelegalcontractapi",
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
"start": "node index.js"
},
"repository": {
"type": "git",
"url": "https://github.com/agavitalis/adobe_legal_contracts.git"
},
"author": "Ogbonna Vitalis",
"license": "ISC",
"bugs": {
"url": "https://github.com/agavitalis/adobe_legal_contracts/issues"
},
"homepage": "https://github.com/agavitalis/adobe_legal_contracts#readme"
}
```

在這些代碼段中，您安裝了應用程式相依性，包括檢視的「控制欄」範本引擎。

此教學課程的主要重點在於使用 [[!DNL Acrobat Services] API](https://developer.adobe.com/document-services/homepage/) 將檔案轉換為 PDF。 因此，建立此Node.js應用程式的步驟不成問題。 不過，您可以在 GitHub[&#128279;](https://github.com/agavitalis/adobe_legal_contracts.git) 上擷取完整的工作Node.js應用程式代碼。

## 將 API 整合 [!DNL Adobe Acrobat Services] 至Node.js應用程式

[!DNL Adobe Acrobat Services] API 是雲端型的可靠服務，專為無縫處理文件而設計。 它提供三個 API：

* Adobe PDF服務API

* Adobe PDF嵌入API

* Adobe檔產生API

您需要認證才能使用 [!DNL Acrobat Services] API （與 PDF 內嵌API認證不同）。 如果您沒有有效的認證， [請依照下方螢幕擷取圖所示，註冊](https://www.adobe.com/go/dcsdks_credentials?ref=getStartedWithServicesSDK) 並完成工作流程。 [享受六個月免費試用，然後按即付費](https://developer.adobe.com/document-services/pricing/main)，每份檔交易只需 $0.05。

![建立新認證的螢幕擷圖](assets/legal_6.png)

註冊程式完成後，程式代碼範例會自動下載到您的PC，以協助您開始。 您可以擷取此程式代碼範例並遵循。 不要忘記將pdftools-api-credentials.json，並將檔案從解壓縮的程式代碼範例中private.key到Node.js專案的根目錄。 您必須先取得憑證，才能存取 [!DNL Acrobat Services] API端點。 您也可以使用個人化認證下載 SDK 範例，這樣您就不必更新範例程式代碼中的密鑰。

現在，使用應用程式根目錄中的終端機執行命令，即可安裝 Adobe PDF Services Node SDK ```npm install \--save @adobe/documentservices-pdftools-node-sdk``` 。 成功安裝後，您可以使用 [!DNL Acrobat Services] API 作應用程式中的檔案。

## 建立 PDF 檔

[!DNL Acrobat Services] API 支援從 Microsoft Office 檔 （Word、Excel 和 PowerPoint） 和其他 [支援的檔案格式](https://opensource.adobe.com/pdftools-sdk-docs/release/latest/howtos.html#create-a-pdf) （例如 .txt、.rtf、.bmp、.jpeg、gif、.tiff 和 .png） 建立 PDF。 您可以使用 Acrobat Service API，輕鬆將法律合約從任何其他檔格式轉換為 PDF。

Adobe檔產生API啟用轉換為 Word 檔案或 PDF。 例如，您可以使用 Word 範本產生合約，包括重新標示以標記編輯過的文字。 然後，將其轉換為 PDF，並使用 PDF Services API以密碼保護檔、傳送以索取簽名等。

若要使用可用的檔案格式來建立 PDF 檔案，您可使用以下格式上傳檔，以便使用 [!DNL Acrobat Services]。

所設計上傳表格會顯示在下方的螢幕擷取畫面中，您可以在 GitHub[&#128279;](https://github.com/agavitalis/adobe_legal_contracts.git) 上存取 HTML 和 CSS 檔案。

![窗體上傳的螢幕擷圖](assets/legal_7.png)

現在，將下列代碼段新增至控制器 /createPDFController.js檔案。 此程式代碼會擷取上傳的檔，並將其轉換為 PDF。 [!DNL Acrobat Services] 將原始上傳的檔案和已轉換的檔案儲存在不同檔案夾中。

```
###controllers/createPDFController.js
const PDFToolsSdk = require('@adobe/documentservices-pdftools-node-sdk');
const Document = require('../models/document');
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
// Create an ExecutionContext using credentials and create a new operation instance.
const executionContext = PDFToolsSdk.ExecutionContext.create(credentials),
createPdfOperation = PDFToolsSdk.CreatePDF.Operation.createNew();
// Set operation input from a source file.
const input = PDFToolsSdk.FileRef.createFromLocalFile(filePath);
createPdfOperation.setInput(input);
// Execute the operation and Save the result to the specified location.
createPdfOperation.execute(executionContext)
.then(async(result) => {
let newFileName = `createPDFFromDOCX-${Math.random() * 171}.pdf`
let newFilePath = require('path').resolve('./') + `\\output\\${newFileName}`
await result.saveAsFile(`views/output/${newFileName}`)
//Creates a new document
let newDocument = new Document({
documentName: newFileName,
url: newFilePath
});
//Save it into the DB.
newDocument.save((err, docs) => {
if (err) {
res.send(err);
}
else {
res.redirect('/?response=PDF Successfully created')
}
});
})
.catch(err => {
if (err instanceof PDFToolsSdk.Error.ServiceApiError
|| err instanceof PDFToolsSdk.Error.ServiceUsageError) {
console.log('Exception encountered while executing operation', err);
} else {
console.log('Exception encountered while executing operation', err);
}
});
} catch (err) {
console.log('Exception encountered while executing operation', err);
}
}
//export all the functions
module.exports = { createPDF, createPDFPost };
```

上述代碼段需要您先前安裝的檔案模型和 [!DNL Acrobat Services] Node SDK。 有兩種功能：

* createPDF 顯示上傳文件表格。

* createPDFPost 會將上傳的文件轉換為 PDF。

這些功能會將已轉換的 PDF 檔案儲存在檢視/輸出目錄中，您可以將檔案下載到電腦。

您也可以使用免費的 PDF 內嵌API來預覽已轉換的 PDF 檔案。 使用 PDF 內嵌 API，您可以在這裡[&#128279;](https://www.adobe.com/go/dcsdks_credentials)產生 Adobe 認證 （與您的[!DNL Acrobat Services]認證不同），並註冊允許的網域以存取 API。請依照程式作，併為您的應用程式產生 PDF 內嵌API認證。 您也可以在這裡觀看示範 [，您可以從中](https://documentcloud.adobe.com/view-sdk-demo/index.html#/view/FULL_WINDOW/Bodea%20Brochure.pdf)輕鬆產生程序代碼，快速入門。

返回應用程式，在應用程式的檢視檔案夾中建立 list.hbs 和 preview.hbs 檔案，然後分別將下方的代碼段貼到 list.hbs 和 preview.hbs 檔案中。

```
###views/list.hbs
<!DOCTYPE html>
<html lang="en">
<head>
<title>Adobe Legal Contract</title>
<!-- Meta tags -->
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,
initial-scale=1.0">
<meta http-equiv="X-UA-Compatible" content="ie=edge">
<!-- //Meta tags -->
<link
href=".min.css" rel="stylesheet" integrity="sha384-eOJMYsd53ii+scO/
bJGFsiCZc+5NDVN2yr8+0RDqr0Ql0h+rP48ckxlpbzKgwra6" crossorigin="anonymous">
<link rel="stylesheet" href="css/style.css" type="text/css"
media="all" /><!-- Style-CSS -->
<link href="css/font-awesome.css" rel="stylesheet" /><!--
font-awesome-icons -->
</head>
<body>
<section>
<div class="form-36-mian section-gap">
<div class="wrapper">
<div class="container">
<div class="row">
{{#each documents}}
<div class="col-md-4 mb-2">
<div class="card" style="width:
18rem;">
<img class="card-img-top"
src="./images/pdf.png"
alt="Card image cap">
<div class="card-body">
<h5
class="card-title">{{documentName}}</h5>
<a
href="/downloadPDF/{{_id}}" class="btn btn-primary"><i class="fa
fa-download" aria-hidden="true"></i> Download</a>
<a
href="/previewPDF/{{_id}}" class="btn btn-info"><i class="fa fa-eye"
aria-hidden="true"></i> Preview</a>
</div>
</div>
</div>
{{/each}}
</div>
</div>
<!-- copyright -->
<div class="copy-right">
<p>(c) 2021 Vitalis</p>
</div>
<!-- //copyright -->
</div>
</div>
</section>
</body>
</html>
###views/preview.hbs
<!DOCTYPE html>
<html lang="en">
<head>
<title>[!DNL Adobe Acrobat Services] PDF Embed API</title>
<meta charset="utf-8" />
<meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1" />
<meta id="viewport" name="viewport" content="width=device-width,
initial-scale=1" />
</head>
<body style="margin: 0px">
<input type="hidden" id="pdfDocumentName"
value={{document.documentName}} />
<input type="hidden" id="pdfDocumentUrl" value={{document.url}} />
<div id="adobe-dc-view"></div>
<script
src="https://documentcloud.adobe.com/view-sdk/main.js"></script>
<script type="text/javascript">
let pdfDocumentName =
document.getElementById("pdfDocumentName").value;
let pdfDocumentUrl =
document.getElementById("pdfDocumentUrl").value;
document.addEventListener("adobe_dc_view_sdk.ready", function
() {
var adobeDCView = new AdobeDC.View({ clientId:
"XXXXXXXXXXXXXXXX", divId: "adobe-dc-view" });
adobeDCView.previewFile({
content: { location: { url:
`http://localhost:5000/output/${pdfDocumentName}` } },
metaData: { fileName: pdfDocumentName }
}, {});
});
</script>
</body>
</html>
```

此外，建立控制器/previewController.js檔案，並將下方的代碼段貼到檔案中。

```
const Document = require('../models/document');
/*
* GET /listFiles route to show PDF file lists.
*/
async function listFiles(req, res) {
let documents = await Document.find({});
res.render('lists', { documents })
}
/*
* GET /previewPDF route to show PDF file in AdobeEmbedAPI.
*/
async function previewPDF(req, res) {
//catch any response on the url
let documentId = req.params.documentId
let document = await Document.findOne({_id:documentId});
res.render('preview', { document })
}
/*
* GET /downloadPDF To Download PDF Documents.
*/
async function downloadPDF(req, res) {
let documentId = req.params.documentId
let document = await Document.findOne({_id:documentId});
res.download(document.url);
}
//export all the functions
module.exports = {listFiles, previewPDF, downloadPDF };
```

在上方的控制器檔案中，有三種功能：listFiles、previewPDF 和 downloadPDF。 listFiles 功能會列出目前使用 [!DNL Acrobat Services] API 產生的所有 PDF 檔案。 previewPDF 功能可讓您使用 PDF 內嵌API來預覽 PDF 檔案，而 downloadPDF 功能則可讓您將產生的 PDF 檔案下載至計算機。 下方的螢幕擷取會顯示使用 PDF 內嵌API的 PDF 預覽範例。

![PDF 預覽螢幕擷圖](assets/legal_8.png)

## 摘要

在此實作教學課程中，您可以使用 Document Generation Tagger Microsoft Word 載入巨集標記檔。 然後，將 API 整合 [!DNL Acrobat Services] 至Node.js應用程式和
將標籤檔轉換為可下載的 PDF 格式，不過您也可以直接將合法合約轉換為 PDF。 最後，您使用「內嵌API Adobe PDF預覽產生的 PDF 以進行驗證和簽署。

完成的應用程式可讓您更輕鬆 [地標記具有動態欄位的合法合約範本](https://developer.adobe.com/document-services/use-cases/agreements-and-contracts/legal-contracts) 、將其轉換為 PDF、預覽及使用 [!DNL Acrobat Services] API 簽署。 您的團隊可以自動將正確的合約傳送給每個客戶，然後有更多時間來發展業務，而不必花時間建立唯一合約。

組織會使用 [!DNL Adobe Acrobat Services] API 來獲得其完整性和易用性。 最棒的，您可以享有 [六個月免費試用版，然後隨時隨](https://developer.adobe.com/document-services/pricing/main)地付費。 您只需為所用內容付費。 此外，PDF 內嵌API一律免費。

準備好透過改善檔流程來提高生產力了嗎？ [立即開始](https://www.adobe.io/apis/documentcloud/dcsdk/gettingstarted.html) 。
